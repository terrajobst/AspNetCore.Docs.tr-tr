---
title: ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage bağımlılıklarıyla desteklenen tüm ASP.NET Core ve Entity Framework Core paketleri içerir.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123833"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage

> [!NOTE]
> ASP.NET Core 2.1 hedefleyen uygulamalar önerilir ve daha sonra [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) bu paket yerine. Bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçirme](#migrate) bu makaledeki.

Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:

* Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.
* Tüm paketleri Entity Framework Core tarafından desteklenmiyor.
* ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.

ASP.NET Core özelliklerinin tümünü 2.x ve Entity Framework Core 2.x dahil edilecek `Microsoft.AspNetCore.All` paket. Bu paket hedefleyen ASP.NET Core 2.0 varsayılan proje şablonları kullanın.

Sürüm numarasını `Microsoft.AspNetCore.All` metapackage sürümü Entity Framework Core ve ASP.NET Core sürümünü temsil eder.

Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından [.NET Core çalışma zamanı Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Çalışma zamanı Store, ASP.NET Core 2.x uygulamaları çalıştırmak için gerekli olan tüm çalışma zamanı varlıkları içerir. Kullanırken `Microsoft.AspNetCore.All` metapackage, **hiçbir** başvurulan bir ASP.NET Core NuGet paket varlıklarından uygulamayla dağıtılan &mdash; .NET Core çalışma zamanı Store bu varlıkları içerir. Uygulama başlatma süresini iyileştirmek için çalışma zamanı Store varlıkları önceden derlenmiş.

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
