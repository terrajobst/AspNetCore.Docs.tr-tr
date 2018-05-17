---
title: Razor görünüm derleme ve ASP.NET Core içinde ön derlemesi
author: rick-anderson
description: MVC Razor görünüm derleme ve ASP.NET Core uygulamalarda ön derlemesi etkinleştirmeyi öğrenin.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Razor dosya (.cshtml) ASP.NET Core derlemede

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Görünüm çağrıldığında razor görünümleri çalışma zamanında derlenir. ASP.NET Core 2.1.0 veya üzeri derleme görünümlerde oluşturmak ve zaman kullanarak yayımlamak [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk). ASP.NET Core 1.1 ve ASP.NET Core 2.0, görünümleri isteğe bağlı olarak Yayımla derlenmiş ve değiştirebilirsiniz uygulamayla dağıtılan &mdash; ön derleme aracını kullanarak. 



Ön derleme dikkate alınacak noktalar:

* Görünümleri önceden derleme, bir küçük yayımlanan paket ve daha hızlı başlangıç saati ile sonuçlanır.
* Görünümleri ön derleme yap sonra Razor dosyaları düzenleyemezsiniz. Düzenlenen görünümleri yayımlanan pakete mevcut olmayacaktır. 

Önceden derlenmiş görünümleri dağıtmak için:

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
Derleme ve Razor dosyaları derlenmesini Razor SDK'sı tarafından varsayılan olarak etkindir zaman yayımlayın. Güncelleştirmeden sonra Razor dosyaları düzenleme derleme zamanında desteklenir. Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir cshtml dosyası uygulamanızla birlikte dağıtılır. 
    
> [!IMPORTANT]
> Razor SDK'sı etkili için hiçbir ön derleme belirli özellikler, proje dosyasında ayarlanır. Örneğin, ayarı `MvcRazorCompileOnPublish` içinde *.csproj* dosya Razor SDK'yı devre dışı bırakır.

# <a name="aspnet-core-20tabaspnetcore20"></a>[ASP.NET Core 2.0](#tab/aspnetcore20/)

Projeniz .NET Framework hedefliyorsa, bir paket başvuru eklemek [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Projeniz .NET Core hedefliyorsa, hiçbir değişiklik gereklidir.

ASP.NET Core 2.x proje şablonları örtük olarak ayarlar `MvcRazorCompileOnPublish` için `true` varsayılan olarak, yani bu düğüm güvenle kaldırılabilir gelen *.csproj* dosya.
    
> [!IMPORTANT]
> Razor görünüm ön derlemesi kullanılamıyor gerektiğinde gerçekleştirme bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0. 

Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI yayımlama komutu](/dotnet/core/tools/dotnet-publish). Örneğin, proje kökündeki aşağıdaki komutu yürütün:

```console
dotnet publish -c Release
```

A *< project_name >. PrecompiledViews.dll* derlenmiş Razor görünümleri içeren dosyası ön derlemesi başarılı olduğunda oluşturulur. Örneğin, aşağıdaki ekran görüntüsünde içeriğini gösterir *Index.cshtml* içine *WebApplication1.PrecompiledViews.dll*:

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Ayarlama `MvcRazorCompileOnPublish` için `true` ve bir paket başvuru eklemek `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Aşağıdaki *.csproj* örnek bu ayarları vurgular:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

