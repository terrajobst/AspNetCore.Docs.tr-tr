---
title: "Mac için Visual Studio Razor sayfalarının uygulamayla bir modeli ekleme"
author: rick-anderson
description: "Mac için Visual Studio kullanarak ASP.NET Core Razor sayfalarının uygulamada bir modeli ekleme"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 53ac6a5a530cdf4e58908f108bcdd0baa66da934
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Mac için Visual Studio ile ASP.NET Core Razor sayfalarının uygulamada bir modeli ekleme

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

* Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**. Klasör adı *modelleri*.
* Sağ *modelleri* klasörünü ve ardından **Ekle** > **yeni dosya**.
* İçinde **yeni dosya** iletişim:

  * Seçin **genel** sol bölmede.
  * Seçin **boş sınıfı** center sorun içinde.
  * Sınıf adını **film** seçip **yeni**.

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Sağ tıklayın örneğin kırmızı kırık çizgi `MovieContext` satırına `services.AddDbContext<MovieContext>(options =>`. Seçin **hızlı düzeltme > RazorPagesMovie.Models; kullanarak**. Visual studio ekler using deyimi.

Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.

![Sayfa oluşturma](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Geçişler için Entity Framework Core NuGet paketleri

EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Tıklayın [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) kullanmak için sürüm numarasını almak için bağlantı. Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* dosya. **Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi.

Düzenlemek için bir *.csproj* dosyası:

* Seçin **dosya** > **açık**ve ardından *.csproj* dosya.
* Seçin **seçenekleri**.
* Değişiklik **birlikte Aç** için **kaynak kod düzenleyicisinde**.

![Csproj dosyasını düzenleyin](model/csproj.png)

Ekleme `Microsoft.EntityFrameworkCore.Tools.DotNet` aracı ikinci referansı  **\<ItemGroup >**.:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

Aşağıdaki kodda gösterildiği sürüm numaralarını yazıldığı sırada doğru.

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Sayfa/filmler dosyaları projeye ekleyin

* Visual Studio'da sağ *sayfaları* klasörü ve seçin **Ekle > varolan klasörü Ekle**.
* Seçin *filmler* klasör.
* İçinde *projeye dahil edilecek Chosse dosyalar* iletişim kutusunda **dahil tüm**.

Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.

>[!div class="step-by-step"]
[Önceki: Başlarken](xref:tutorials/razor-pages-mac/razor-pages-start)
[sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages-mac/page)
