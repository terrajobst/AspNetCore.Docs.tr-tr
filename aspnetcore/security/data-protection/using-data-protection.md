---
title: ASP.NET core'da veri koruma API'lerini kullanmaya başlama
author: rick-anderson
description: Koruma ve uygulama veri koruması kaldırıldıktan için ASP.NET Core veri koruma API'lerini kullanmayı öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087632"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>ASP.NET core'da veri koruma API'lerini kullanmaya başlama

<a name="security-data-protection-getting-started"></a>

Basit ve koruma verilerini aşağıdaki adımlardan oluşur:

1. Veri koruyucu bir veri koruma sağlayıcısı oluşturun.

2. Çağrı `Protect` yöntemi ile korumak istediğiniz verileri.

3. Çağrı `Unprotect` düz metin yeniden etkinleştirmek istediğiniz veri yöntemi.

Çoğu çerçeveleri ve ASP.NET Core veya SignalR, gibi uygulama modelleri zaten veri koruma sisteminde yapılandırın ve bağımlılık ekleme erişim hizmet kapsayıcı ekleyin. Aşağıdaki örnek, bir hizmet kapsayıcısı için bağımlılık ekleme ve veri koruma yığın kaydetme, DI aracılığıyla veri koruma sağlayıcısı alma, koruyucusu ve veri koruma sonra koruması kaldırılıyor oluşturma gösterilmektedir.

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Bir koruyucu oluşturduğunuzda, bir veya daha fazla sağlamalısınız [amaç dizeleri](xref:security/data-protection/consumer-apis/purpose-strings). Amaç dize tüketicileri arasında yalıtım sağlar. Örneğin, "green" amaçlı dizisi ile oluşturulan bir koruyucu "mor" bir amacı bir koruyucu ile sağlanan verilerin korumasını saptayamazdınız.

>[!TIP]
> Örneklerini `IDataProtectionProvider` ve `IDataProtector` olan birden çok arayanlar için iş parçacığı açısından güvenli. Bir bileşen için bir başvuru alır. sonra istemiş bir `IDataProtector` çağrısıyla `CreateProtector`, birden çok çağrı için bu başvuru kullanacağı `Protect` ve `Unprotect`.
>
>Bir çağrı `Unprotect` korumalı yükü doğrulandı veya yararlanılarak CryptographicException oluşturmaz. Bazı bileşenler hataları yoksayma isteyebilirsiniz sırasında işlemleri; Korumasını Kaldır kimlik doğrulaması tanımlama bilgileri okuyan bir bileşen bu hatayı işlemek ve isteği, tanımlama bilgisi hiç varmış gibi davran yerine yükseltebilir istek başarısız. Bu davranış istediğiniz bileşenleri, tüm özel durumları swallowing yerine CryptographicException özel olarak yakalamalısınız.
