---
ms.openlocfilehash: 9ac005f3381e02ad31c0b1b87f8d55a05cc0b6cb
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265311"
---
<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

* Komut satırından aşağıdaki komutu çalıştırın (içeren proje dizininde *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları):

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Hatası alırsanız:

  ```
  No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Proje dizini için bir komut kabuğunu açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
