---
title: ASP.NET core'da veri koruma için dı kullanmayan senaryolar
author: rick-anderson
description: Burada olamaz veya bağımlılık ekleme tarafından sağlanan bir hizmet kullanmak istemiyorsanız veri koruma senaryolarını desteklemek üzere öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 62280a9f911b003383cbe348b9b62942766a2b99
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048233"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>ASP.NET core'da veri koruma için dı kullanmayan senaryolar

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core veri koruma normalde sistemidir [hizmet kapsayıcıya eklenen](xref:security/data-protection/consumer-apis/overview) ve bağımlılık ekleme (dı) aracılığıyla bağımlı bileşenler tarafından kullanılır. Ancak, burada bu değildir uygun ya da istediğiniz durumlar vardır özellikle sistem var olan bir uygulamayı içeri aktarılırken.

Bu senaryoları desteklemek için [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paket sağlar somut bir türde [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), veri korumayı kullanması için basit bir yol sunar DI üzerinde bağlı olmadan. `DataProtectionProvider` Yazın uygular [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Oluşturma `DataProtectionProvider` yalnızca sağlanmasını gerektirir. bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) sağlayıcıya ait şifreleme anahtarlarını depolanması gereken yere, aşağıdaki kod örneğinde görüldüğü gibi göstermek için örnek:

[!code-csharp[](non-di-scenarios/_static/nodisample1.cs)]

Varsayılan olarak, `DataProtectionProvider` somut bir türde değil ham anahtar malzemesi şifreleme dosya sistemi için önce kalıcı. Burada bir ağ paylaşımı ve veri koruma sisteminde Geliştirici noktalara uygun bekleyen bir anahtar şifreleme mekanizması otomatik olarak çıkarılamıyor senaryolarını desteklemek üzere budur.

Ayrıca, `DataProtectionProvider` somut bir türde değil [Ayır uygulamalar](xref:security/data-protection/configuration/overview#per-application-isolation) varsayılan olarak. Anahtar ile aynı dizine kullanarak tüm uygulamaları yüklerini sürece paylaşabilirsiniz kendi [amaç parametreleri](xref:security/data-protection/consumer-apis/purpose-strings) eşleşmesi.

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) Oluşturucusu sistem davranışlarını ayarlamak için kullanılan isteğe bağlı yapılandırma geri çağırma kabul eder. Aşağıdaki örnekte, geri yükleme için açık çağrı yalıtımıyla gösterir [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). Örnek ayrıca, sistem otomatik olarak Windows DPAPI kullanarak kalıcı anahtarlarını şifrelemek için yapılandırma gösterir. Dizini için bir UNC paylaşımı işaret ediyorsa, paylaşılan sertifika ilgili tüm makinelerde dağıtmak ve sertifika tabanlı şifreleme çağrısı ile kullanmak için sistemi yapılandırmak için hazırlandığını [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-csharp[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Örneklerini `DataProtectionProvider` somut türünü oluşturmak pahalı. Bir uygulama birden çok örneğini bu tür tutar ve tümünü aynı dizinde anahtar depolama alanı kullanıyorsanız, uygulama performansı düşebilir. Kullanırsanız `DataProtectionProvider` türü öneririz bu tür bir kez oluşturun ve mümkün olduğunca yeniden. `DataProtectionProvider` Türü ve tüm [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) oluşturulan iş parçacığı açısından güvenli örnekleridir birden çok arayanlar için.
