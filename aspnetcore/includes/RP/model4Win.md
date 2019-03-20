---
ms.openlocfilehash: 7fb5e956264f3cc7c04c4023ce69a058e5d0c0ed
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265364"
---
<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

* Komut satırından aşağıdaki komutu çalıştırın (içeren proje dizininde *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları):

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Hatası alırsanız:

  ```
  No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Önceki hatayı yanlış dizine olduğunda gerçekleşir. Proje dizini için bir komut kabuğunu açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları), ve ardından yukarıdaki komutu çalıştırın.

Hatası alırsanız:

  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

Visual Studio çıkın ve komutu yeniden çalıştırın.
