---
title: ASP.NET Core için tüketici API 'Lerine genel bakış
author: rick-anderson
description: ASP.NET Core veri koruma kitaplığı 'nda bulunan çeşitli tüketici API 'Lerine ilişkin kısa bir genel bakış alın.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666588"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>ASP.NET Core için tüketici API 'Lerine genel bakış

`IDataProtectionProvider` ve `IDataProtector` arabirimleri, tüketicilerin veri koruma sistemini kullanan temel arabirimlerdir. Bunlar [Microsoft. AspNetCore. DataProtection. soyutlamalar](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) paketinde bulunur.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Sağlayıcı arabirimi, veri koruma sisteminin kökünü temsil eder. Verileri korumak veya korumayı kaldırmak için doğrudan kullanılamaz. Bunun yerine, amaç `IDataProtectionProvider.CreateProtector(purpose)`çağırarak bir `IDataProtector` başvurusunu almalıdır; burada amaç hedeflenen tüketicinin kullanım durumunu açıklayan bir dizedir. Bu parametrenin amacı hakkında daha fazla bilgi ve uygun bir değer seçme hakkında daha fazla bilgi için bkz. [Amaç dizeleri](xref:security/data-protection/consumer-apis/purpose-strings) .

## <a name="idataprotector"></a>IDataProtector

Koruyucu arabirimi bir `CreateProtector`çağrısıyla döndürülür ve bu arabirim, tüketicilerin koruma ve kaldırma işlemleri gerçekleştirmek için kullanabileceği bu arabirimdir.

Bir veri parçasını korumak için verileri `Protect` metoduna geçirin. Temel arabirim, Byte []-> Byte [] ' ı dönüştüren bir yöntemi tanımlar, ancak dize > dizesini dönüştüren aşırı yükleme (uzantı yöntemi olarak sağlanmış) de vardır. İki yöntem tarafından sunulan güvenlik aynıdır; geliştirici, kullanım durumu için hangi aşırı yükün en kullanışlı olduğunu seçmelidir. Seçilen aşırı yükleme ne olursa olsun, koruma yöntemi tarafından döndürülen değer artık korunur (şifreleme ve üzerinde oynanarak denetlenen) ve uygulama onu güvenilmeyen bir istemciye gönderebilir.

Daha önce korunan bir veri parçasının korumasını kaldırmak için korunan verileri `Unprotect` metoduna geçirin. (Geliştiriciye kolaylık olması için Byte [] tabanlı ve dize tabanlı aşırı yüklemeler vardır.) Korumalı yük bu aynı `IDataProtector`daha önceki bir `Protect` çağrısıyla oluşturulduysa `Unprotect` yöntemi, orijinal korumasız yükü döndürür. Korunan yük üzerinde oynanmış veya farklı bir `IDataProtector`üretildiyse, `Unprotect` yöntemi CryptographicException oluşturur.

Aynı ve farklı `IDataProtector` kavramı, amaç kavramıyla geri benzer. Aynı kök `IDataProtectionProvider`, ancak `IDataProtectionProvider.CreateProtector`çağrısındaki farklı amaç dizeleri aracılığıyla iki `IDataProtector` örneği oluşturulduysa, [farklı koruyucular](xref:security/data-protection/consumer-apis/purpose-strings)olarak değerlendirilir ve diğeri tarafından oluşturulan yüklerin korumasını kaldırılamaz.

## <a name="consuming-these-interfaces"></a>Bu arabirimleri kullanma

Dı kullanan bir bileşen için, istenen kullanım, bileşenin oluşturucusunda bir `IDataProtectionProvider` parametresi aldığı ve bileşen örneklendirirken dı sisteminin otomatik olarak bu hizmeti sağladığını sunmasıdır.

> [!NOTE]
> Bazı uygulamalar (konsol uygulamaları veya ASP.NET 4. x uygulamaları gibi), burada açıklanan mekanizmayı kullanamaz. Bu senaryolar için, bir `IDataProtection` sağlayıcısının örneğini vermeksizin alma hakkında daha fazla bilgi için, bu [olmayan duyarlı senaryolar](xref:security/data-protection/configuration/non-di-scenarios) belgesine başvurun.

Aşağıdaki örnek üç kavram göstermektedir:

1. [Veri koruma sistemini](xref:security/data-protection/configuration/overview) hizmet kapsayıcısına ekleme,

2. Bir `IDataProtectionProvider`örneğini almak için DI kullanma ve

3. Bir `IDataProtectionProvider` `IDataProtector` oluşturma ve verileri korumak ve korumayı kaldırmak için kullanma.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Microsoft. AspNetCore. DataProtection. soyutlamalar paketi geliştirici kolaylığı olarak `IServiceProvider.GetDataProtector` bir genişletme yöntemi içerir. Hizmet sağlayıcısından bir `IDataProtectionProvider` alarak ve `IDataProtectionProvider.CreateProtector`çağırarak tek bir işlem olarak kapsüller. Aşağıdaki örnek kullanımını gösterir.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> `IDataProtectionProvider` ve `IDataProtector` örnekleri, birden çok çağıranlar için iş parçacığı güvenlidir. Bir bileşen `CreateProtector`çağrısı aracılığıyla bir `IDataProtector` başvuru aldığında, bu başvuruyu `Protect` ve `Unprotect`birden çok çağrı için kullanacaktır. Korunan yük doğrulanamazsa veya çözülemez bir `Unprotect` çağrısı CryptographicException oluşturur. Bazı bileşenler, kaldırma işlemleri sırasında hataları yoksaymak isteyebilir; kimlik doğrulama tanımlama bilgilerini okuyan bir bileşen bu hatayı işleyebilir ve isteği, isteğin hemen başarısız olması yerine hiç bir tanımlama bilgisine sahip olmamış gibi değerlendirir. Bu davranışın, tüm özel durumlara izin vermek yerine CryptographicException özel olarak yakalamalı bileşenler.
