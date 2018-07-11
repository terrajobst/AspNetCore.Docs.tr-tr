---
title: Mac için Visual Studio ile ASP.NET Core Razor sayfalar uygulama için model ekleme
author: rick-anderson
description: Bir model bir Mac için Visual Studio kullanarak ASP.NET Core Razor sayfaları uygulamasına eklemeyi öğrenin
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186764"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>Mac için Visual Studio ile ASP.NET Core Razor sayfalar uygulama için model ekleme

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

* Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**. Klasör adı *modelleri*.
* Sağ *modelleri* klasöre tıklayın ve ardından **Ekle** > **yeni dosya**.
* İçinde **yeni dosya** iletişim:

  * Seçin **genel** sol bölmesinde.
  * Seçin **boş sınıf** içinde merkezi kolaylaştırın.
  * Sınıf adı **film** seçip **yeni**.

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Sağ tıklayın örneğin kırmızı dalgalı çizgi `MovieContext` satırında `services.AddDbContext<MovieContext>(options =>`. Seçin **hızlı düzeltme > RazorPagesMovie.Models; kullanarak**. Visual studio ekler using deyimi.

Herhangi bir hata yoksa doğrulamak için projeyi derleyin.

![sayfası oluşturma](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Geçişler için Entity Framework Core NuGet paketleri

EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Tıklayarak [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) kullanmak için sürüm numarasını almak için bağlantı. Bu paketi yüklemek için eklemeniz `DotNetCliToolReference` koleksiyonda *.csproj* dosya. **Not:** düzenleyerek bu paketi yüklemek sahip olduğunuz *.csproj* dosya; kullanamazsınız `install-package` komut veya Paket Yöneticisi GUI.

Düzenlenecek bir *.csproj* dosyası:

* Seçin **dosya** > **açık**ve ardından *.csproj* dosya.
* Seçin **seçenekleri**.
* Değişiklik **birlikte Aç** için **Kaynak Kod Düzenleyicisi**.

![Csproj dosyasını düzenleyin](model/csproj.png)

Ekleme `Microsoft.EntityFrameworkCore.Tools.DotNet` aracı ikinci başvuru  **\<ItemGroup >**.:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

Aşağıdaki kodda gösterilen sürüm numaraları makalenin yazıldığı sırada, doğru.

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Sayfa/film dosyaları projeye Ekle

* Visual Studio'da sağ *sayfaları* klasörü ve select **Ekle > mevcut klasörü Ekle**.
* Seçin *filmler* klasör.
* İçinde *projeye eklenecek dosyaları seçin* iletişim kutusunda **dahil tüm**.

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages-mac/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages-mac/page)
