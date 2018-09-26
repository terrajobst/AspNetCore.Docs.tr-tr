---
title: Microsoft.AspNetCore.App metapackage ASP.NET Core 2.1 ve üzeri
author: Rick-Anderson
description: Microsoft.AspNetCore.App metapackage desteklenen tüm ASP.NET Core ve Entity Framework Core paketleri içerir.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 68b5aca60273a8c6ef03c0a29842e6a5305adeb3
ms.sourcegitcommit: 517bb1366da2a28b0014e384fa379755c21b47d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47230171"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>ASP.NET Core 2.1 için Microsoft.AspNetCore.App metapackage

Bu özellik, ASP.NET Core 2.1 ve üzeri hedefleyen .NET Core 2.1 ve üzeri gerektirir.

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) ASP.NET Core için:

* Üçüncü taraf bağımlılıklarının dışında içermez [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), ve [IX zaman uyumsuz](https://www.nuget.org/packages/System.Interactive.Async/). Bu 3. taraf bağımlılıkları, ana çerçeveler özellikleri işlevi sağlamak için gerekli sayılan.
* Üçüncü taraf bağımlılıkları (dışında daha önce bahsedilenler) içeren hariç ASP.NET Core ekibi tarafından desteklenen tüm paketleri içerir.
* Üçüncü taraf bağımlılıkları (dışında daha önce bahsedilenler) içeren hariç Entity Framework Core ekibi tarafından desteklenen tüm paketleri içerir.

Tüm özellikleri ASP.NET Core 2.1 ve üzeri ve Entity Framework Core 2.1 ve üzeri dahil edilen `Microsoft.AspNetCore.App` paket. Varsayılan proje şablonları ASP.NET Core 2.1 hedefleme ve daha sonra bu paketi kullanın. ASP.NET Core 2.1 ve üzeri ve Entity Framework Core 2.1 hedefleyen uygulamalar önerilir ve daha sonra `Microsoft.AspNetCore.App` paket.

Sürüm numarasını `Microsoft.AspNetCore.App` metapackage sürümü Entity Framework Core ve ASP.NET Core sürümünü temsil eder.

Kullanarak `Microsoft.AspNetCore.App` metapackage uygulamanızı korumak sürüm kısıtlamaları sağlar:

* Bir paket eklenirse, bağımlıdır geçişli (doğrudan değil) bir paket içinde `Microsoft.AspNetCore.App`ve bu sürüm numaraları farklılık gösterir, NuGet bir hata üretir.
* Uygulamanıza eklediğiniz diğer paketleri dahil paketleri sürümünü değiştiremezsiniz `Microsoft.AspNetCore.App`.
* Sürüm tutarlılık, güvenilir bir deneyim sağlar. `Microsoft.AspNetCore.App` ilgili BITS birlikte aynı uygulamada kullanılan test edilmemiş sürümü bileşimleri önlemek için tasarlanmıştır.

Kullanan uygulamalar `Microsoft.AspNetCore.App` metapackage otomatik olarak ASP.NET Core paylaşılan çerçeve avantajlarından yararlanın. Kullanırken `Microsoft.AspNetCore.App` metapackage, **hiçbir** başvurulan bir ASP.NET Core NuGet paket varlıklarından uygulamayla dağıtılan&mdash;ASP.NET Core paylaşılan çerçeve bu varlıkları içerir. Uygulama başlatma süresini kısaltmak için paylaşılan çerçevesindeki varlıkları önceden derlenmiş. Daha fazla bilgi için bkz: "paylaşılan çerçeve" [.NET Core dağıtımı paketleme](/dotnet/core/build/distribution-packaging).

Şu proje dosya başvuruları `Microsoft.AspNetCore.App` metapackage ASP.NET Core ve tipik bir ASP.NET Core 2.1 temsil şablonu için:

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

Önceki biçimlendirme, normal ASP.NET Core 2.1 ve üzeri şablonu temsil eder. İçin bir sürüm numarası belirtmeyen `Microsoft.AspNetCore.App` paket başvurusu. Version belirtilmediğinde, bir [örtük](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) sürümü belirtildi SDK tarafından diğer bir deyişle, `Microsoft.NET.Sdk.Web`. SDK'sı tarafından belirtilen sürüm numarası paket başvurusu üzerinde açıkça ayarlamak örtük sürümü güvenmek öneririz. Bu yaklaşım hakkında sorularınız varsa, GitHub yorum [Microsoft.AspNetCore.App örtük sürümü için tartışma](https://github.com/aspnet/Docs/issues/6430).

Örtük sürüm kümesine `major.minor.0` taşınabilir uygulamalar için. Paylaşılan çerçeve sarma mekanizması, uygulama, en son uyumlu sürümü yüklü paylaşılan çerçeveleri arasında çalıştırılır. Aynı sürüm, geliştirme, test ve üretim kullanılan sağlamak için paylaşılan framework sürümüyle aynı sürümü, tüm ortamlara yüklenen emin olun. Kendi uygulamaları'için örtük bir sürüm numarası ayarlanır `major.minor.patch` yüklü SDK'yı paketlenmiş paylaşılan framework'ün.

Sürüm numarasını belirtme `Microsoft.AspNetCore.App` başvuru yapar **değil** paylaşılan sürümünün garanti framework seçilebilir. Örneğin, sürümü "2.1.1" belirtildi, ancak "2.1.3" yüklü olduğunu varsayalım. Bu durumda, uygulama "2.1.3" kullanır. Önerilmemesine rağmen ileri sarma (düzeltme eki ve/veya ikincil) devre dışı bırakabilirsiniz. Dotnet konak sarma ve davranışını yapılandırma hakkında daha fazla bilgi için bkz. [dotnet konak ileri sarma](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

`<Project Sdk` ayarlanmalıdır `Microsoft.NET.Sdk.Web` örtük sürümünü kullanacak şekilde `Microsoft.AspNetCore.App`.  Zaman `<Project Sdk="Microsoft.NET.Sdk">` (sondaki olmadan `.Web`) kullanılır:

* Aşağıdaki uyarısı oluşturulur:

     *Uyarı NU1604: Proje bağımlılığı Microsoft.AspNetCore.App kapsamlı bir alt sınırı içermiyor. Alt sınır tutarlı geri yükleme sonuçları emin olmak için bağımlılık sürümünü içerir.*
* Bu, .NET Core 2.1 SDK'sı ile bilinen bir sorundur ve .NET Core 2.2 SDK düzeltilecektir.

<a name="update"></a>

## <a name="update-aspnet-core"></a>ASP.NET Core güncelleştir

`Microsoft.AspNetCore.App` [Metapackage](/dotnet/core/packages#metapackages) Nuget'ten güncelleştirilir geleneksel bir paket değil. Benzer şekilde `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` NuGet dışında işlenmiş özel sürüm semantiğe sahip bir paylaşılan çalışma zamanı, temsil eder. Daha fazla bilgi için [paketler, meta paketler ve çerçeveler](/dotnet/core/packages).

ASP.NET Core güncelleştirmek için:

* Geliştirme makineler ve yapı sunucularını: indirme ve yükleme [.NET Core SDK'sı](https://www.microsoft.com/net/download).
* Dağıtım sunucularında: indirme ve yükleme [.NET Core çalışma zamanı](https://www.microsoft.com/net/download).

 Uygulamalar için en son yüklenen sürüm uygulama başlatmada İleri dökümünü yapar. Güncelleştirme gerekli değil `Microsoft.AspNetCore.App` proje dosyasındaki sürüm numarası. Daha fazla bilgi için [Framework bağımlı uygulamaları alma İleri](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Uygulamanızı daha önce kullandıysanız `Microsoft.AspNetCore.All`, bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçirme](xref:fundamentals/metapackage#migrate).
