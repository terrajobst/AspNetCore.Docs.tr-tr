---
title: ASP.NET Core 2,0 için Microsoft. AspNetCore. All metapackage
author: Rick-Anderson
description: Microsoft. AspNetCore. All metapackage, ASP.NET Core 2,1 ve üzeri için önerilmez.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: cc00c075909da5c17a4aa2fd252c9e662e5a0fc9
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511073"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>ASP.NET Core 2,0 için Microsoft. AspNetCore. All metapackage

::: moniker range=">= aspnetcore-3.0"

`Microsoft.AspNetCore.All` metapackage, ASP.NET Core 3,0 ve sonraki sürümlere dahil değildir. Daha fazla bilgi için [Bu GitHub sorununa](https://github.com/aspnet/Announcements/issues/314)bakın.

::: moniker-end

> [!NOTE]
> ASP.NET Core 2,1 ' i hedefleyen uygulamaların ve daha sonra bu paket yerine [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) kullanılmasını öneririz. Bkz. Bu makaledeki [Microsoft. AspNetCore. All Ile Microsoft. aspnetcore. app 'e geçme](#migrate) .

Bu özellik, .NET Core 2. x 'i hedefleyen ASP.NET Core 2. x gerektirir.

[Microsoft. AspNetCore. All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) , paylaşılan bir çerçeveye başvuran bir metapackage. *Paylaşılan çerçeve* , uygulamanın klasörlerinde olmayan derlemelerin ( *. dll* dosyaları) bir kümesidir. Uygulamayı çalıştırmak için makinede paylaşılan çerçeve yüklü olmalıdır. Daha fazla bilgi için bkz. [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

`Microsoft.AspNetCore.All` başvurduğu paylaşılan çerçeve şunları içerir:

* ASP.NET Core ekibi tarafından desteklenen tüm paketler.
* Entity Framework Core tarafından desteklenen tüm paketler.
* ASP.NET Core ve Entity Framework Core tarafından kullanılan dahili ve üçüncü taraf bağımlılıklar.

ASP.NET Core 2. x ve Entity Framework Core 2. x ' in tüm özellikleri `Microsoft.AspNetCore.All` paketine dahildir. ASP.NET Core 2,0 ' i hedefleyen varsayılan proje şablonları bu paketi kullanır.

`Microsoft.AspNetCore.All` metapackage sürüm numarası, en düşük ASP.NET Core sürümü ve Entity Framework Core sürümünü temsil eder.

Aşağıdaki *. csproj* dosyası, ASP.NET Core için `Microsoft.AspNetCore.All` metapackage 'e başvurur:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Örtük sürüm oluşturma

ASP.NET Core 2,1 veya üzeri sürümlerde, bir sürüm olmadan `Microsoft.AspNetCore.All` paketi başvurusunu belirtebilirsiniz. Sürüm belirtilmediğinde, SDK tarafından örtük bir sürüm belirtilir (`Microsoft.NET.Sdk.Web`). SDK tarafından belirtilen örtük sürüme güvenmek ve paket başvurusunda sürüm numarasını açıkça ayarlamamanız önerilir. Bu yaklaşım hakkında sorularınız varsa, [Microsoft. AspNetCore. app örtük sürümü Için tartışmada](https://github.com/dotnet/AspNetCore.Docs/issues/6430)bir GitHub yorumu bırakın.

Örtük sürüm, taşınabilir uygulamalar için `major.minor.0` olarak ayarlanır. Paylaşılan Framework toplaması-iletme mekanizması, uygulamayı yüklü paylaşılan Çerçeveler arasındaki en son uyumlu sürümde çalıştırır. Geliştirme, test ve üretimde aynı sürümün kullanıldığını güvence altına almak için, paylaşılan Framework 'ün aynı sürümünün tüm ortamlarda yüklü olduğundan emin olun. Kendi içinde bulunan uygulamalar için, örtük sürüm numarası, yüklü SDK 'da paketlenmiş paylaşılan Framework `major.minor.patch` ayarlanır.

`Microsoft.AspNetCore.All` paketi başvurusunda bir sürüm numarası belirtilmesi, paylaşılan Çerçeve sürümünün seçili olduğunu garanti **etmez** . Örneğin, "2.1.1" sürümünün belirtildiğini, ancak "2.1.3" nin yüklü olduğunu varsayalım. Bu durumda, uygulama "2.1.3" kullanacaktır. Önerilmese de, iletmeyi (Patch ve/veya Minor) devre dışı bırakabilirsiniz. DotNet ana bilgisayar alma hakkında daha fazla bilgi ve davranışını yapılandırma hakkında daha fazla bilgi için bkz. [DotNet Host top Forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Projenin SDK 'sının, `Microsoft.AspNetCore.All`örtük sürümünü kullanabilmesi için proje dosyasında `Microsoft.NET.Sdk.Web` olarak ayarlanması gerekir. `Microsoft.NET.Sdk` SDK belirtildiğinde (proje dosyasının en üstünde`<Project Sdk="Microsoft.NET.Sdk">`), aşağıdaki uyarı oluşturulur:

*Uyarı NU1604: proje bağımlılığı Microsoft. AspNetCore. All, kapsamlı bir alt sınır içermez. Tutarlı geri yükleme sonuçlarının sağlanması için bağımlılık sürümüne bir alt sınır ekleyin.*

Bu, .NET Core 2,1 SDK ile ilgili bilinen bir sorundur ve .NET Core 2,2 SDK 'sında düzeltilecektir.

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Microsoft. AspNetCore. All 'dan Microsoft. AspNetCore. app 'e geçiş

Aşağıdaki paketler `Microsoft.AspNetCore.All`, `Microsoft.AspNetCore.App` paketine dahil değildir.

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

`Microsoft.AspNetCore.All` ' den `Microsoft.AspNetCore.App`' ye taşımak için, uygulamanız yukarıdaki paketlerdeki API 'Leri veya bu paketler tarafından getirilen paketleri kullanıyorsa, projenizdeki bu paketlere başvurular ekleyin.

Önceki paketlerin, aksi durumda `Microsoft.AspNetCore.App` bağımlılıkları olmayan tüm bağımlılıkları örtük olarak dahil edilmez. Örnek:

* `Microsoft.Extensions.Caching.Redis` bağımlılığı olarak `StackExchange.Redis`
* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup` bağımlılığı olarak `Microsoft.ApplicationInsights`

## <a name="update-aspnet-core-21"></a>Güncelleştirme ASP.NET Core 2,1

2,1 ve üzeri için `Microsoft.AspNetCore.App` metapackage 'e geçiş yapmanızı öneririz. `Microsoft.AspNetCore.All` metapackage kullanmaya devam etmek ve en son düzeltme eki sürümünün dağıtıldığından emin olmak için:

* Geliştirme makinelerinde ve yapı sunucularında: en son [.NET Core SDK](https://dotnet.microsoft.com/download)yükler.
* Dağıtım sunucularında: en son [.NET Core çalışma zamanını](https://dotnet.microsoft.com/download)yükler.
 Uygulamanız, uygulama yeniden başlatıldığında en son yüklenen sürüme ileri alınacaktır.
