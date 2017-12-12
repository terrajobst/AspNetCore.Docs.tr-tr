---
title: "Tüketici API'leri genel bakış"
author: rick-anderson
description: "Bu belge, çeşitli tüketici API'leri ASP.NET Core veri koruma kitaplıkta kullanılabilir kısa bir genel bakış sağlar."
keywords: "ASP.NET Core, veri koruma tüketici API'leri"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: c80ed22776b0dbbaccb686f8c2ea534e6f5d9a74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="consumer-apis-overview"></a>Tüketici API'leri genel bakış

`IDataProtectionProvider` Ve `IDataProtector` arabirimlerdir temel arabirimleri tüketiciler veri koruma sistemi kullanır. İçinde bulunan [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) paket.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Sağlayıcı arabirimi veri koruma sisteminde kökünü temsil eder. Koruma veya verilerin korumasını doğrudan kullanılamaz. Bunun yerine, bir başvuru tüketici almalısınız bir `IDataProtector` çağırarak `IDataProtectionProvider.CreateProtector(purpose)`amacı hedeflenen tüketici kullanım örneğini tanımlayan bir dize olmalıdır. Bkz: [amacı dizeleri](purpose-strings.md) Bu parametre ve uygun bir değer seçme hedefi üzerinde çok daha fazla bilgi için.

## <a name="idataprotector"></a>Idataprotector

Koruyucu arabirimi için bir çağrı tarafından döndürülen `CreateProtector`, ve tüketicileri gerçekleştirmek için kullanabileceğiniz bu arabirim korumak ve işlemleri korumasını kaldırın.

Veri parçası korumak için verileri geçirmek `Protect` yöntemi. Hangi dönüştürür byte [] -> byte [] bir yöntem temel arabirimi tanımlar, ancak yok de dize dönüştüren (bir genişletme yöntemi sağlanır) bir aşırı -> dize. İki yöntem tarafından sunulan güvenlik aynıdır; Geliştirici, hangi aşırı kendi kullanım durumu için en uygun seçmeniz gerekir. Seçilen, aşırı koruma tarafından döndürülen değer yedeklemiş yöntemi şimdi (enciphered ve yetkisiz değiştirmeye karşı proofed) korunur ve uygulama güvenilmeyen bir istemci gönderebilirsiniz.

Daha önce korunan bir veri korumasını kaldırmak için korumalı veri geçirmek `Unprotect` yöntemi. (Byte [] vardır-tabanlı ve dize tabanlı aşırı geliştiriciye kolaylık sağlamak için.) Korumalı yükü için önceki bir çağrı tarafından oluşturulmuşsa `Protect` bu aynı üzerinde `IDataProtector`, `Unprotect` yöntemi özgün korumasız yükü döndürür. Korumalı yükü oynanmış veya farklı bir tarafından üretilmiş `IDataProtector`, `Unprotect` yöntemi CryptographicException oluşturur.

Kavramı aynı, farklı karşılaştırması `IDataProtector` TIES amacı kavramı yedekleyin. İki `IDataProtector` örnekleri aynı kökünden oluşturuldu `IDataProtectionProvider` ancak farklı amaç dizeleri çağrısında aracılığıyla `IDataProtectionProvider.CreateProtector`, kabul edilen sonra [farklı koruyucuları](purpose-strings.md), ve bir edemeyecek korumasını kaldırmak tarafından oluşturulan yükler.

## <a name="consuming-these-interfaces"></a>Bu arabirimleri kullanma

DI algılayan bir bileşen için bileşen yapmanızı hedeflenen kullanım olan bir `IDataProtectionProvider` kurucusu parametresinde ve bileşen örneği oluşturulduğunda dı sistem otomatik olarak bu hizmet sağlar.

> [!NOTE]
> Bazı uygulamalar (örneğin, konsol uygulaması veya ASP.NET 4.x uygulamaları) dı açıklanan mekanizması burada kullanamazlar uyumlu olmayabilir. Bu senaryolar başvurun [olmayan dı kullanan senaryolar](../configuration/non-di-scenarios.md) belge örneği alma hakkında daha fazla bilgi için bir `IDataProtection` dı giderek olmadan sağlayıcısı.

Aşağıdaki örnek, üç kavramları göstermektedir:

1. [Veri koruma sisteminde ekleme](../configuration/overview.md) hizmet kapsayıcısı

2. Örneğini almak için dı kullanarak bir `IDataProtectionProvider`, ve

3. Oluşturma bir `IDataProtector` gelen bir `IDataProtectionProvider` ve korumak ve veri korumasını kaldırmak için kullanma.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Bir genişletme yöntemi Microsoft.AspNetCore.DataProtection.Abstractions paketi içeren `IServiceProvider.GetDataProtector` Geliştirici kolaylık. Her iki alma tek bir işlem olarak yalıtan bir `IDataProtectionProvider` hizmet sağlayıcısı ve arama `IDataProtectionProvider.CreateProtector`. Aşağıdaki örnek kullanımını gösterir.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Örneklerini `IDataProtectionProvider` ve `IDataProtector` olan birden çok çağıranlar için iş parçacığı. Bu bileşen bir başvuru edinir sonra hedeflenen bir `IDataProtector` çağrısıyla `CreateProtector`, birden çok çağrılar için bu başvuru kullanacağı `Protect` ve `Unprotect`. Çağrı `Unprotect` korumalı yükü doğrulandı veya yararlanılarak CryptographicException özel durum oluşturacak. Bazı bileşenler hatalarını yok saymak isteyebilirsiniz sırasında işlemleri; korumayı Kaldır kimlik doğrulaması tanımlama bilgileri okuyan bir bileşeni bu hatayı işleyebilir ve isteği, tanımlama bilgisi hiç sahipmiş gibi ele alın yerine depolayabileceği isteği başarısız. Bu davranış istediğiniz bileşenleri tüm özel durumları swallowing yerine CryptographicException özellikle catch.
