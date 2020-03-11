---
title: ASP.NET Core 'de veri koruma API 'Lerini kullanmaya başlayın
author: rick-anderson
description: Bir uygulamadaki verileri korumak ve korumayı kaldırmak için ASP.NET Core veri koruma API 'Lerini kullanmayı öğrenin.
ms.author: riande
ms.date: 11/12/2019
no-loc:
- SignalR
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 8c3f3c7fb21434cf335591c41741f0ce868df33e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664439"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>ASP.NET Core 'de veri koruma API 'Lerini kullanmaya başlayın

<a name="security-data-protection-getting-started"></a>

En basit olan verileri korumak aşağıdaki adımlardan oluşur:

1. Bir veri koruma sağlayıcısından veri koruyucusu oluşturun.

2. `Protect` yöntemini korumak istediğiniz verilerle çağırın.

3. `Unprotect` yöntemini, düz metin haline döndürmek istediğiniz verilerle çağırın.

ASP.NET Core veya SignalRgibi çoğu çerçeve ve uygulama modelleri, veri koruma sistemini zaten yapılandırıp bağımlılık ekleme yoluyla erişenbir hizmet kapsayıcısına ekler. Aşağıdaki örnek, bağımlılık ekleme ve veri koruma yığınını kaydetme, veri koruma sağlayıcısını dı aracılığıyla alma, bir koruyucu oluşturma ve verilerin korumasını kaldırma için bir hizmet kapsayıcısını yapılandırmayı gösterir.

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Bir koruyucu oluşturduğunuzda bir veya daha fazla [Amaç dizesi](xref:security/data-protection/consumer-apis/purpose-strings)sağlamanız gerekir. Amaç dizesi, tüketiciler arasında yalıtım sağlar. Örneğin, "yeşil" bir amaç dizesiyle oluşturulan bir koruyucu, "mor" amacını taşıyan bir koruyucu tarafından belirtilen verilerin korumasını yapamaz.

>[!TIP]
> `IDataProtectionProvider` ve `IDataProtector` örnekleri, birden çok çağıranlar için iş parçacığı güvenlidir. Bir bileşen `CreateProtector`çağrısı aracılığıyla bir `IDataProtector` başvuru aldığında, bu başvuruyu `Protect` ve `Unprotect`birden çok çağrı için kullanacaktır.
>
>Korunan yük doğrulanamazsa veya çözülemez bir `Unprotect` çağrısı CryptographicException oluşturur. Bazı bileşenler, kaldırma işlemleri sırasında hataları yoksaymak isteyebilir; kimlik doğrulama tanımlama bilgilerini okuyan bir bileşen bu hatayı işleyebilir ve isteği, isteğin hemen başarısız olması yerine hiç bir tanımlama bilgisine sahip olmamış gibi değerlendirir. Bu davranışın, tüm özel durumlara izin vermek yerine CryptographicException özel olarak yakalamalı bileşenler.
