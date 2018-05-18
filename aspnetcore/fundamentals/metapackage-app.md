---
title: Microsoft.AspNetCore.App metapackage ASP.NET Core 2.1 ve üzeri
author: Rick-Anderson
description: Desteklenen tüm ASP.NET Core ve Entity Framework Çekirdek paket Microsoft.AspNetCore.App metapackage içerir.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage-app
ms.openlocfilehash: ac402bf8542560267ae179a1f41212380435dbfa
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>ASP.NET Core 2.1 Microsoft.AspNetCore.App metapackage

Bu özellik, ASP.NET Core 2.1 ve daha sonra .NET Core 2.1 hedefleme ve üstü gerektirir.

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) metapackage ASP.NET Core için:

* Üçüncü taraf bağımlılıkları dışında içermez [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), ve [IX zaman uyumsuz](https://www.nuget.org/packages/System.Interactive.Async/). Bu 3. taraf bağımlılıkları ana çerçeveleri özellikleri işlevi emin olmak gerekli olarak kabul edilen.
* Üçüncü taraf bağımlılıkları (dışında olanlar daha önce bahsedilen) içeren olanlar dışında ASP.NET Core ekibi tarafından desteklenen tüm paketleri içerir.
* Üçüncü taraf bağımlılıkları (dışında olanlar daha önce bahsedilen) içeren olanlar dışında Entity Framework Çekirdek ekibi tarafından desteklenen tüm paketleri içerir.

Tüm özellikleri ASP.NET Core 2.1 ve üzeri ve Entity Framework Çekirdek 2.1 ve üzeri dahil edilen `Microsoft.AspNetCore.App` paket. Varsayılan proje şablonları ASP.NET Core 2.1 hedefleme ve daha sonra bu paketi kullanabilirsiniz. ASP.NET Core 2.1 ve üzeri ve Entity Framework Çekirdek 2.1 hedefleyen uygulamalar önerilir ve daha sonra kullanmak `Microsoft.AspNetCore.App` paket.

Sürüm numarasını `Microsoft.AspNetCore.App` ASP.NET Core sürüm ve Entity Framework Çekirdek sürüm metapackage temsil eder.

Kullanarak `Microsoft.AspNetCore.App` metapackage uygulamanızı korumak sürüm kısıtlamaları sağlar:

* Bir paket eklenirse, bir geçişli (doğrudan değil) bağımlılığa sahip bir pakette üzerinde `Microsoft.AspNetCore.App`ve bu sürüm numaraları farklılık, NuGet, bir hata üretir.
* Uygulamanıza eklediğiniz diğer paketleri dahil paketlerinin sürümüne değiştiremezsiniz `Microsoft.AspNetCore.App`.
* Sürüm tutarlılığı güvenilir bir deneyim sağlar. `Microsoft.AspNetCore.App` aynı uygulamada birlikte kullanılan ilgili BITS sınanmamış sürümü bileşimleri önlemek üzere tasarlanmıştır.

Kullanan uygulamalar `Microsoft.AspNetCore.App` metapackage otomatik olarak ASP.NET Core paylaşılan çerçevesinden alın. Kullandığınızda `Microsoft.AspNetCore.App` metapackage, **hiçbir** başvurulan ASP.NET Core NuGet paketlerini varlıklarından uygulama ile dağıtılan &mdash; ASP.NET Core paylaşılan framework bu varlıkları içerir. Uygulama başlangıç zamanını geliştirmek için paylaşılan framework varlıkları önceden derlenmiş. Daha fazla bilgi için bkz: "paylaşılan framework" [.NET Core dağıtım paketleme](/dotnet/core/build/distribution-packaging).

Aşağıdaki *.csproj* proje dosyası başvuruları `Microsoft.AspNetCore.App` metapackage ASP.NET Core için:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

Önceki biçimlendirme tipik ASP.NET Core 2.1 ve üzeri şablonu temsil eder. İçin sürüm numarasını belirtmeyen `Microsoft.AspNetCore.App` paketini başvuru. Sürüm belirtilmediğinde örtük bir sürüm olarak SDK tarafından diğer bir deyişle, belirtilen `Microsoft.NET.Sdk.Web`. SDK'sı tarafından belirtilen ve açıkça sürüm numarasını üzerinde paket başvuru ayarlama örtük sürümü güvenmek öneririz. Bir GitHub yorum bırakabilir [Microsoft.AspNetCore.App örtük sürümü için tartışma](https://github.com/aspnet/Docs/issues/6430).

Örtük sürüm kümesine `major.minor.0` taşınabilir uygulamalar için. Paylaşılan framework İleri alma mekanizması uygulama uyumlu en son sürümü yüklü paylaşılan çerçeveleri arasında çalışır. Aynı sürüm, geliştirme, test ve üretim kullanılır güvence altına almak için her ortamda yüklü paylaşılan framework aynı sürümü emin olun. Kendi kendine bulunan uygulamalar için örtük sürüm numarası ayarlanır `major.minor.patch` yüklü SDK'ın paketlenmiş paylaşılan framework'ün.

Bir sürüm numarası belirtme `Microsoft.AspNetCore.App` başvuru etmez **değil** paylaşılan sürümünün garanti framework seçilebilir. Örneğin, sürüm "2.1.1" belirtildi, ancak "2.1.3" yüklü varsayalım. Bu durumda, uygulama "2.1.3" kullanır. Önerilmemesine rağmen Top İleri (düzeltme eki ve/veya ikincil) devre dışı bırakabilirsiniz. Dotnet konak İleri alma ve davranışını yapılandırma ile ilgili daha fazla bilgi için bkz: [dotnet konak İleri alma](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

Uygulamanızı daha önce kullandıysanız `Microsoft.AspNetCore.All`, bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçiş](xref:fundamentals/metapackage#migrate).