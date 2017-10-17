---
title: "Bir model, bir ASP.NET Core MVC uygulamasına ekleniyor."
author: rick-anderson
description: "Bir model için basit bir ASP.NET Core uygulama ekleyin."
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: "ASP.NET Core, Webapı, Web API'si, REST, Mac, Linux, HTTP, hizmeti, HTTP hizmeti, VS Code"
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Bir sınıfa eklemek *modelleri* adlı klasörü *Movie.cs*.
* Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Alan veritabanı için birincil anahtarı gerekli. 

Herhangi bir hata yoktur ve son olarak eklediğiniz doğrulamak üzere uygulamasını derleme bir **M**odel için **M**VC uygulama.

## <a name="prepare-the-project-for-scaffolding"></a>Proje için askılamayı hazırlama

- Aşağıdaki vurgulanmış şekilde NuGet paketlerini eklemek *MvcMovie.csproj* dosyası:
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Dosyayı kaydedin ve seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.
- Oluşturma bir *Models/MvcMovieContext.cs* dosya ve aşağıdakileri ekleyin `MvcMovieContext` sınıfı:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Açık *haline* dosya ve iki kullanımları ekleyin:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Veritabanı bağlamı Ekle *haline* dosyası:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Bu, hangi modeli sınıfları veri modelinde bulunan Entity Framework bildirir. Bir tanımladığınız *varlık kümesini* veritabanında bir filmi tablo olarak temsil edilir film nesnelerin.

- Bir hata doğrulamak için projeyi oluşturun.

## <a name="scaffold-the-moviecontroller"></a>İskele MovieController

Proje klasöründe bir terminal penceresi açın ve aşağıdaki komutları çalıştırın:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> Yapı iskelesi komutu çalıştığında bir hata alırsanız, bkz: [444 yapı iskelesi deposunda sorun](https://github.com/aspnet/scaffolding/issues/444) geçici bir çözüm için.

Yapı iskelesi altyapısı aşağıdaki oluşturur:

* Film denetleyicisi (*Controllers/MoviesController.cs*)
* Razor Görünüm Oluştur, Sil, ayrıntıları, düzenleme ve dizin sayfalar için dosyaları (*görünümler/filmler/\*.cshtml*)

Otomatik olarak oluşturulmasını [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve görünümler olarak bilinir *iskele*. En kısa sürede bir filmi veritabanı yönetmenizi sağlayan bir tam olarak işlevsel bir web uygulaması gerekir.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Bir veritabanı ve görüntüleme, düzenleme, güncelleştirme ve verileri silmek için sayfaları artık sahipsiniz. Sonraki öğreticide biz veritabanı ile çalışması.

### <a name="additional-resources"></a>Ek kaynaklar

* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)
* [Genelleştirme ve yerelleştirme](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Önceki - Görünüm ekleme](adding-view.md)
[sonraki - SQLite ile çalışma](working-with-sql.md)
