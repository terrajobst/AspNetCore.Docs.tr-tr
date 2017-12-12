---
title: "Anahtar girişi ve ayarlarını değiştirme"
author: rick-anderson
description: "Bu belge ASP.NET Core veri koruma anahtarı girişi API'leri uygulama ayrıntılarını özetlemektedir."
keywords: "ASP.NET Core, veri koruma, anahtar girişi"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 96860b44b64f241a1bbff2ac8366e0863b1ac10c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="key-immutability-and-changing-settings"></a>Anahtar girişi ve ayarlarını değiştirme

Yedekleme deposu için bir nesneyi kalıcı sonra kendi gösterimi sonsuza kadar düzeltilmiştir. Yeni veri yedekleme deposu eklenebilir, ancak var olan verileri asla dönüşür. Bu davranış birincil amacı, veri bozulması engellemektir.

Bu davranış bir sonucu bir anahtarı yedekleme deposu yazıldıktan sonra değişmez olmasıdır. İptal olsa kullanarak kendi oluşturma, etkinleştirme ve sona erme tarihleri hiçbir zaman, değiştirilebilir `IKeyManager`. Ayrıca, temel alınan algoritmik bilgileri, ana anahtar malzemesini ve geri kalan özellikleri şifreleme de değişmez.

Geliştirici anahtar Kalıcılık etkileyen herhangi bir ayar değişirse, bu değişiklikleri yürürlüğe bir anahtar oluşturulur, zamana kadar için açık bir çağrı aracılığıyla ya da gidecek değil `IKeyManager.CreateNewKey` veya veri koruma sisteminin kendi aracılığıyla [otomatik anahtarı nesil](key-management.md#data-protection-implementation-key-management) davranışı. Anahtar Kalıcılık etkileyen ayarlar aşağıdaki gibidir:

* [Varsayılan anahtar yaşam süresi](key-management.md#data-protection-implementation-key-management)

* [Anahtar şifreleme rest mekanizması](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Anahtarı içinde yer alan algoritmik bilgileri](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Bu ayarların zaman çalışırken sonraki otomatik anahtarı'den önceki kazandırın gerekiyorsa, açık bir çağrı yapmayı düşünün `IKeyManager.CreateNewKey` yeni bir anahtar oluşturma zorlamak için. Bir açık etkinleştirme tarihi vermeyi unutmayın ({şimdi + 2 gün} bir iyi değişikliğin yayılması zaman tanıyın için udur) ve çağrısında sona erme tarihi.

>[!TIP]
> Depo temas tüm uygulamalar aynı ayarlarla belirtmelisiniz `IDataProtectionBuilder` genişletme yöntemleri. Aksi takdirde, kalıcı anahtar özelliklerini anahtar oluşturma yordamları çağrılan belirli uygulamanın bağımlı olur.
