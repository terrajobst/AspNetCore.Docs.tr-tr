---
title: ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage
author: Rick-Anderson
description: ASP.NET Core 2.1 ve üzeri Microsoft.AspNetCore.All metapackage önerilmez.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148856"
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

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Örtük sürüm oluşturma

ASP.NET Core 2.1 veya daha sonra belirtebilirsiniz `Microsoft.AspNetCore.All` paketini başvuru olmadan bir sürüm. Version belirtilmediğinde, SDK tarafından belirtilen örtük bir sürüm (`Microsoft.NET.Sdk.Web`). SDK'sı tarafından belirtilen sürüm numarası paket başvurusu üzerinde açıkça ayarlamak örtük sürümü güvenmek öneririz. Bu yaklaşım hakkında sorularınız varsa, GitHub yorum [Microsoft.AspNetCore.App örtük sürümü için tartışma](https://github.com/aspnet/Docs/issues/6430).

Örtük sürüm kümesine `major.minor.0` taşınabilir uygulamalar için. Paylaşılan çerçeve sarma mekanizması uygulamanın en yeni uyumlu sürümü yüklü paylaşılan çerçeveleri arasında çalışır. Aynı sürüm, geliştirme, test ve üretim kullanılan sağlamak için paylaşılan framework sürümüyle aynı sürümü, tüm ortamlara yüklenen emin olun. Bağımsız uygulamalar için örtük bir sürüm numarası ayarlanır `major.minor.patch` yüklü SDK'yı paketlenmiş paylaşılan framework'ün.

Sürüm numarasını belirtme `Microsoft.AspNetCore.All` paket başvurusu mu **değil** paylaşılan sürümünün garanti framework seçilir. Örneğin, sürümü "2.1.1" belirtildi, ancak "2.1.3" yüklü olduğunu varsayalım. Bu durumda, uygulama "2.1.3" kullanır. Önerilmemesine rağmen ileri sarma (düzeltme eki ve/veya ikincil) devre dışı bırakabilirsiniz. Dotnet konak sarma ve davranışını yapılandırma hakkında daha fazla bilgi için bkz. [dotnet konak ileri sarma](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Projenin SDK ayarlanmalıdır `Microsoft.NET.Sdk.Web` örtük sürümünü kullanmak için proje dosyasında `Microsoft.AspNetCore.All`. Zaman `Microsoft.NET.Sdk` SDK belirtildi (`<Project Sdk="Microsoft.NET.Sdk">` proje dosyasının üst), aşağıdaki uyarısı oluşturulur:

*Uyarı NU1604: Proje bağımlılığı Microsoft.AspNetCore.All kapsamlı bir alt sınırı içermiyor. Alt sınır tutarlı geri yükleme sonuçları emin olmak için bağımlılık sürümünü içerir.*

Bu, .NET Core 2.1 SDK'sı ile bilinen bir sorundur ve .NET Core 2.2 SDK düzeltilecektir.

::: moniker-end

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
