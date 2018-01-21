---
title: "ASP.NET Çekirdeği'nde veri koruma için olmayan dı kullanan senaryolar"
author: rick-anderson
description: "Burada olamaz veya bağımlılık ekleme tarafından sağlanan bir hizmet kullanmak istemiyorsanız veri koruma senaryoları desteklemek öğrenin."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 1c84cfcf44086359a7d6900ca52781dc6f3b1b10
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>ASP.NET Çekirdeği'nde veri koruma için olmayan dı kullanan senaryolar

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core veri koruması normalde sistemidir [bir hizmet kapsayıcısı eklenen](xref:security/data-protection/consumer-apis/overview) ve bağımlılık ekleme (dı) üzerinden bağımlı bileşenleri tarafından tüketilen. Ancak, burada bu değildir uygun ya da istediğiniz durumlar vardır özellikle sistem varolan bir uygulamaya alırken.

Bu senaryoları desteklemek için [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paket sağlar somut bir türde [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), veri korumayı kullanması için basit bir yol sunar DI üzerinde bağlı olmadan. `DataProtectionProvider` Yazın uygulayan [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Oluşturma `DataProtectionProvider` yalnızca sağlama gerektiren bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) sağlayıcının şifreleme anahtarları depolanacağı, aşağıdaki kod örneğinde görüldüğü gibi göstermek için örnek:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

Varsayılan olarak, `DataProtectionProvider` somut türde değil raw anahtar malzemesini şifreleme dosya sistemi önce kalıcı. Burada bir ağ paylaşımı ve veri koruması sistem Geliştirici noktalarına otomatik olarak bir rest en uygun anahtar şifrelemesi mekanizması türetme yapamazsınız senaryoları desteklemek için budur.

Ayrıca, `DataProtectionProvider` somut türde değil [yalıtmak uygulamaları](xref:security/data-protection/configuration/overview#per-application-isolation) varsayılan olarak. Anahtar ile aynı dizinde kullanan tüm uygulamalar yüklerini sürece paylaşabilirsiniz kendi [amaçla parametreleri](xref:security/data-protection/consumer-apis/purpose-strings) eşleşmesi.

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) Oluşturucusu sistemi davranışlarını ayarlamak için kullanılan isteğe bağlı yapılandırma geri çağırma kabul eder. Aşağıdaki örneği için açık bir çağrı geri yükleme yalıtımıyla gösterir [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). Örnek, otomatik olarak Windows DPAPI kullanarak kalıcı anahtarları şifrelemek için sistem yapılandırma da gösterir. Dizin bir UNC paylaşımı işaret ediyorsa, paylaşılan bir sertifikası tüm ilgili makinelerde dağıtmak ve sistemi çağrısıyla sertifika tabanlı şifreleme kullanacak biçimde yapılandırmak için isteyebilir [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Örneklerini `DataProtectionProvider` somut türde oluşturmak pahalı. Bir uygulama bu türdeki birden çok örneği koruyorsa ve bunların tümü aynı anahtar depolama dizini kullanıyorsanız, uygulama performansının düşmesine neden. Kullanırsanız `DataProtectionProvider` türü öneririz bu tür bir kez oluşturmak ve mümkün olduğunca yeniden. `DataProtectionProvider` Türü ve tüm [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) oluşturulan örnekleri iş parçacığı için birden çok arayanlar.
