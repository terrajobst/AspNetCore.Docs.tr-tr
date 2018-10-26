---
title: ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage
author: Rick-Anderson
description: ASP.NET Core 2.1 ve üzeri Microsoft.AspNetCore.All metapackage önerilmez.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/metapackage
ms.openlocfilehash: f78684cb31976f976aec5e1773bcc728dfecc82e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090712"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage

> [!NOTE]
> ASP.NET Core 2.1 hedefleyen uygulamalar önerilir ve daha sonra [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) bu paket yerine. Bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçirme](#migrate) bu makaledeki.

Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:

* Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.
* Tüm paketleri Entity Framework Core tarafından desteklenmiyor.
* ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.

ASP.NET Core özelliklerinin tümünü 2.x ve Entity Framework Core 2.x dahil edilecek `Microsoft.AspNetCore.All` paket. Bu paket hedefleyen ASP.NET Core 2.0 varsayılan proje şablonları kullanın.

Sürüm numarasını `Microsoft.AspNetCore.All` metapackage sürümü Entity Framework Core ve ASP.NET Core sürümünü temsil eder.

Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından [.NET Core çalışma zamanı Store](/dotnet/core/deploying/runtime-store). Çalışma zamanı Store, ASP.NET Core 2.x uygulamaları çalıştırmak için gerekli olan tüm çalışma zamanı varlıkları içerir. Kullanırken `Microsoft.AspNetCore.All` metapackage, **hiçbir** başvurulan bir ASP.NET Core NuGet paket varlıklarından uygulamayla dağıtılan &mdash; .NET Core çalışma zamanı Store bu varlıkları içerir. Uygulama başlatma süresini iyileştirmek için çalışma zamanı Store varlıkları önceden derlenmiş.

Paket kesme işlemi kullanmadığınız paketlerini kaldırmak için kullanabilirsiniz. Yayımlanmış uygulama çıktıda kırpılmış paketler bırakılır.

Aşağıdaki *.csproj* dosya başvuruları `Microsoft.AspNetCore.All` metapackage ASP.NET Core için:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Microsoft.AspNetCore.All Microsoft.AspNetCore.App için geçirme

Aşağıdaki paketler dahil `Microsoft.AspNetCore.All` ama `Microsoft.AspNetCore.App` paket. 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

Taşıma için `Microsoft.AspNetCore.All` için `Microsoft.AspNetCore.App`yukarıdaki paketlerinden herhangi bir API uygulamanızın kullandığı veya paketleri getirilir, bu paketleri tarafından bu paketleri başvuruları projenize ekleyin.

Aksi takdirde, bağımlılıkları olmayan yukarıdaki paketlerin herhangi bir bağımlılığın `Microsoft.AspNetCore.App` örtük olarak dahil edilmez. Örneğin:

* `StackExchange.Redis` bir bağımlılık olarak `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` bir bağımlılık olarak `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`

## <a name="update-aspnet-core-21"></a>ASP.NET Core 2.1 güncelleştirmesi

Geçiş öneririz `Microsoft.AspNetCore.App` metapackage 2.1 ve üzeri. Kullanmaya devam etmek için `Microsoft.AspNetCore.All` metapackage ve en son düzeltme eki sürümü dağıtıldığı emin olun:

* Geliştirme makineler ve yapı sunucularını: en yeni [.NET Core SDK'sı](https://www.microsoft.com/net/download).
* Dağıtım sunucularında: en yeni [.NET Core çalışma zamanı](https://www.microsoft.com/net/download).
 Uygulamanız için en son yüklenen sürüm üzerinde uygulama yeniden başlatma İleri dökümünü yapar.
