---
ms.openlocfilehash: 648ed8087c64eaf80fd055fa7dd08041ab9a8752
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320299"
---
Kimlik iskele kurucu çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.
* Sol bölmeden **İskele Ekle** iletişim kutusunda **kimlik** > **ekleme**.
* İçinde **ADD kimliğini** iletişim kutusunda, istediğiniz seçenekleri seçin.
  * Varolan bir düzen sayfası seçin veya hatalı biçimlendirme Düzen dosyanızın üzerine yazılacak. Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfaları için `~/Views/Shared/_Layout.cshtml` MVC projeleri
  * Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.
* Seçin **ekleme**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Paket başvurusu ekleme [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projesine (\*.csproj) dosyası. Proje dizininde aşağıdaki komutu çalıştırın:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:

```cli
dotnet aspnet-codegenerator identity -h
```

Proje klasöründe kimlik iskele kurucu istediğiniz seçeneği ile çalıştırın. Örneğin, varsayılan UI kimliği ve en az sayıda dosya Kurulumu için aşağıdaki komutu çalıştırın:

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
