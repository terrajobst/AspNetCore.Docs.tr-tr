---
ms.openlocfilehash: 9ac005f3381e02ad31c0b1b87f8d55a05cc0b6cb
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265311"
---
<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="bd8c3-101">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="bd8c3-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="bd8c3-102">Komut satırından aşağıdaki komutu çalıştırın (içeren proje dizininde *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları):</span><span class="sxs-lookup"><span data-stu-id="bd8c3-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="bd8c3-103">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="bd8c3-103">If you get the error:</span></span>

  ```
  No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="bd8c3-104">Proje dizini için bir komut kabuğunu açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="bd8c3-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
