---
title: Microsoft.AspNetCore.All metapackage ASP.NET Core 2.0 ve üzeri
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage bağımlılıklarını yanı sıra tüm desteklenen ASP.NET Core ve Entity Framework Çekirdek paketleri içerir.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: 2fddc59d74dce4b114b5b4ed0646f773eb66ffb9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277831"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage

> [!NOTE]
> ASP.NET Core 2.1 hedefleme uygulamaları önerilir ve daha sonra kullanmak [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) yerine bu paketi. Bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçiş](#migrate) bu makalede.

Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET Core metapackage içerir:

* Tüm paketler ASP.NET Core ekibi tarafından desteklenir.
* Tüm paketleri Entity Framework Çekirdek tarafından desteklenir. 
* ASP.NET Core ve Entity Framework Çekirdek tarafından kullanılan iç ve 3. taraf bağımlılıkları. 

ASP.NET Core tüm özelliklerini 2.x ve Entity Framework Çekirdek 2.x dahil edilmiştir `Microsoft.AspNetCore.All` paket. Bu paket ASP.NET Core 2.0 hedefleme varsayılan proje şablonları kullanın.

Sürüm numarasını `Microsoft.AspNetCore.All` ASP.NET Core sürüm ve Entity Framework Çekirdek sürüm metapackage temsil eder.

Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından [.NET çekirdeği çalışma zamanı deposu](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Çalışma zamanı deposu ASP.NET Core 2.x uygulamaları çalıştırmak için gereken tüm çalışma zamanı varlıklarını içerir. Kullandığınızda `Microsoft.AspNetCore.All` metapackage, **hiçbir** başvurulan ASP.NET Core NuGet paketlerini varlıklarından uygulama ile dağıtılan &mdash; .NET çekirdeği çalışma zamanı deposu bu varlıkları içerir. Çalışma zamanı deposundaki varlıklar uygulama başlangıç zamanını geliştirmek için önceden derlenmiş.

Paket kesme işlemi kullanmadığınız paketleri kaldırmak için kullanabilirsiniz. Yayımlanan uygulama çıktıda bölünen paketleri bırakılır.

Aşağıdaki *.csproj* dosya başvuruları `Microsoft.AspNetCore.All` metapackage ASP.NET Core için:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Microsoft.AspNetCore.All Microsoft.AspNetCore.App için geçirme

Aşağıdaki paketler içinde yer alan `Microsoft.AspNetCore.All` ama `Microsoft.AspNetCore.App` paket. 

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

Sunucudan taşımak için `Microsoft.AspNetCore.All` için `Microsoft.AspNetCore.App`, yukarıdaki paketlerinden herhangi bir API uygulamanızı kullanıyorsa veya paketler getirildi bu paketleri tarafından bu Paketlerine yönelik başvuruları projenize ekleyin.

Aksi takdirde bağımlılıkları olmayan önceki paket bağımlılıkları `Microsoft.AspNetCore.App` örtük olarak dahil edilmez. Örneğin:

* `StackExchange.Redis` bir bağımlılık olarak `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` bir bağımlılık olarak `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
