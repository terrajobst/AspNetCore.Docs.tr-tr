---
title: ASP.NET Core 'de veri koruması için yok kullanmayan senaryolar
author: rick-anderson
description: Bağımlılık ekleme tarafından sağlanmış bir hizmeti kullanmak istemediğiniz veya kullanmak istemediğiniz veri koruma senaryolarını nasıl destekleyeceğinizi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 62280a9f911b003383cbe348b9b62942766a2b99
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666238"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>ASP.NET Core 'de veri koruması için yok kullanmayan senaryolar

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core Data Protection sistemi normalde [bir hizmet kapsayıcısına eklenir](xref:security/data-protection/consumer-apis/overview) ve bağımlılık ekleme (dı) yoluyla bağımlı bileşenler tarafından kullanılır. Ancak, özellikle de sistem mevcut bir uygulamaya aktarılırken bunun uygulanabilir veya istenen durumlar vardır.

Bu senaryoları desteklemek için, [Microsoft. AspNetCore. DataProtection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paketi, bir somut tür olan [dataprotectionprovider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider)sağlar ve bu da, dı 'ye bağlı olmadan veri koruma kullanmanın basit bir yolunu sunar. `DataProtectionProvider` türü [ıdataprotectionprovider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider)'ı uygular. Yalnızca `DataProtectionProvider`, aşağıdaki kod örneğinde görüldüğü gibi, sağlayıcının şifreleme anahtarlarının nerede depolanacağını belirtmek için bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) örneği sağlanması gerekir:

[!code-csharp[](non-di-scenarios/_static/nodisample1.cs)]

Varsayılan olarak `DataProtectionProvider` somut tür, ham anahtar malzemesini dosya sistemine kalıcı yapmadan önce şifrelemez. Bu, geliştiricinin bir ağ paylaşımının gösterdiği ve veri koruma sisteminin uygun bir rest anahtar şifreleme mekanizmasını otomatik olarak kaldırabildiği senaryoları destekler.

Ayrıca, `DataProtectionProvider` somut tür uygulamaları varsayılan olarak [yalımaz](xref:security/data-protection/configuration/overview#per-application-isolation) . Aynı anahtar dizinini kullanan tüm uygulamalar, kendi [Amaç parametreleri](xref:security/data-protection/consumer-apis/purpose-strings) eşleştiği sürece yükleri paylaşabilir.

[Dataprotectionprovider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) Oluşturucusu, sistemin davranışlarını ayarlamak için kullanılabilen isteğe bağlı bir yapılandırma geri aramasını kabul eder. Aşağıdaki örnek, bir [Setapplicationname](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)öğesine açık bir çağrı ile yalıtımı geri yüklemeyi gösterir. Örnek ayrıca, Windows DPAPI kullanarak kalıcı anahtarları otomatik olarak şifrelemek için sistemi yapılandırmayı gösterir. Dizin bir UNC paylaşımının işaret ediyorsa, tüm ilgili makinelerde paylaşılan bir sertifikayı dağıtmak ve sistemi, [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)çağrısıyla sertifika tabanlı şifrelemeyi kullanacak şekilde yapılandırmak isteyebilirsiniz.

[!code-csharp[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> `DataProtectionProvider` somut türün örnekleri oluşturmak pahalıdır. Bir uygulama bu türün birden fazla örneğini tutar ve hepsi aynı anahtar depolama dizinini kullanıyorsa, uygulama performansı düşebilir. `DataProtectionProvider` türünü kullanırsanız, bu türü bir kez oluşturup mümkün olduğunca yeniden kullanmanız önerilir. `DataProtectionProvider` türü ve bundan oluşturulan tüm [ıdataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) örnekleri birden çok çağıranlar için iş parçacığı güvenlidir.
