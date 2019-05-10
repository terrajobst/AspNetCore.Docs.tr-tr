---
title: Tüketici API'lerine genel bakış için ASP.NET Core
author: rick-anderson
description: ASP.NET Core veri koruma kitaplığı içinde mevcut API'lere çeşitli tüketici kısa bir genel bakış alırsınız.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: b0d11d097ee2d448b6781f6fa84445f6400fbc76
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087549"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Tüketici API'lerine genel bakış için ASP.NET Core

`IDataProtectionProvider` Ve `IDataProtector` arabirimdir temel arabirimler tüketiciler veri koruma sisteminde kullanır. Bulunan [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) paket.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Sağlayıcı arabirimi, veri koruma sisteminin kökünü temsil eder. Doğrudan dosya korumak veya veri korumasını kaldırmak için de kullanılamaz. Bunun yerine, tüketici bir başvuru almak gerekir bir `IDataProtector` çağırarak `IDataProtectionProvider.CreateProtector(purpose)`amaçlı tüketici hedeflenen kullanım örneğini tanımlayan bir dize olduğu. Bkz: [amaç dizeleri](xref:security/data-protection/consumer-apis/purpose-strings) amaç çok daha fazla bilgi bu parametre ve uygun bir değer seçin.

## <a name="idataprotector"></a>IDataProtector

Koruyucu arabirimi için bir çağrı tarafından döndürülen `CreateProtector`ve tüketiciler gerçekleştirmek için kullanabileceğiniz bu arabirimi korumak ve korumasını operations.

Bir veri parçasını korumak için verileri geçirmek `Protect` yöntemi. Hangi dönüştürür byte [] -> byte [] bir yöntem temel bir arabirim tanımlar, ancak var olan dize dönüştürür (bir genişletme yöntemi sağlanan) bir aşırı dize ayrıca ->. İki yöntem tarafından sunulan güvenlik aynıdır; Geliştirici, hangi aşırı kullanım örnekleri için en uygun seçmeniz gerekir. Seçilen, aşırı yükleme koruma tarafından döndürülen değer bakılmaksızın yöntemi artık (enciphered ve kurcalamaya haricindeki) korunur ve uygulama güvenilmeyen bir istemciye göndermek için.

Daha önce korunan bir veri korumasını kaldırmak için korunan verileri geçirmek `Unprotect` yöntemi. (Byte [] vardır-tabanlı ve dize tabanlı aşırı geliştiriciye kolaylık sağlamak için.) Korumalı yükü daha önceki bir çağrı tarafından oluşturulmuşsa `Protect` bu aynı üzerinde `IDataProtector`, `Unprotect` özgün korumasız yükü yöntemi döndürür. Korumalı yükü oynanmış veya farklı bir tarafından üretilen `IDataProtector`, `Unprotect` CryptographicException yöntemi oluşturur.

Aynı kavram farklı karşılaştırması `IDataProtector` TIES amaçlı kavramını yedekleme. İki `IDataProtector` örnekleri aynı kökünden oluşturuldu `IDataProtectionProvider` ancak farklı bir amaç dizeleri çağrısında aracılığıyla `IDataProtectionProvider.CreateProtector`, kabul edilmeleri sonra [farklı koruyucuları](xref:security/data-protection/consumer-apis/purpose-strings), ve bir olmayacaktır korumasını kaldırmak için tarafından oluşturulan yükler.

## <a name="consuming-these-interfaces"></a>Bu arabirimler kullanma

DI algılayan bir bileşen için bileşen yapmanızı hedeflenen kullanım olduğu bir `IDataProtectionProvider` oluşturucusuna parametre ve bileşen örneği oluşturulduğunda DI sistemi bu hizmeti otomatik olarak sağlar.

> [!NOTE]
> Bazı uygulamalar (örneğin konsol uygulamaları veya ASP.NET 4.x uygulamaları) DI burada açıklanan mekanizması kullanamazlar uyumlu olmayabilir. Bu senaryolar doldurulamayabilir [olmayan dı kullanmayan senaryolar](xref:security/data-protection/configuration/non-di-scenarios) belge örneği alma hakkında daha fazla bilgi için bir `IDataProtection` DI giderek olmadan sağlayıcısı.

Aşağıdaki örnek, üç kavramı gösterir:

1. [Veri koruma sisteminde ekleme](xref:security/data-protection/configuration/overview) hizmet kapsayıcısı

2. DI örneğini almak üzere kullanarak bir `IDataProtectionProvider`, ve

3. Oluşturma bir `IDataProtector` gelen bir `IDataProtectionProvider` ve korumaya ve veri korumasını kaldırmak için kullanma.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

' % S'paketi Microsoft.AspNetCore.DataProtection.Abstractions bir genişletme yöntemi içeren `IServiceProvider.GetDataProtector` Geliştirici kolaylık. Hem bir alma tek bir işlem olarak kapsülleyen bir `IDataProtectionProvider` hizmet sağlayıcısı ve arama `IDataProtectionProvider.CreateProtector`. Aşağıdaki örnek, kullanımını gösterir.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Örneklerini `IDataProtectionProvider` ve `IDataProtector` olan birden çok arayanlar için iş parçacığı açısından güvenli. Bir bileşen için bir başvuru alır. sonra istemiş bir `IDataProtector` çağrısıyla `CreateProtector`, birden çok çağrı için bu başvuru kullanacağı `Protect` ve `Unprotect`. Bir çağrı `Unprotect` korumalı yükü doğrulandı veya yararlanılarak CryptographicException oluşturmaz. Bazı bileşenler hataları yoksayma isteyebilirsiniz sırasında işlemleri; Korumasını Kaldır kimlik doğrulaması tanımlama bilgileri okuyan bir bileşen bu hatayı işlemek ve isteği, tanımlama bilgisi hiç varmış gibi davran yerine yükseltebilir istek başarısız. Bu davranış istediğiniz bileşenleri, tüm özel durumları swallowing yerine CryptographicException özel olarak yakalamalısınız.
