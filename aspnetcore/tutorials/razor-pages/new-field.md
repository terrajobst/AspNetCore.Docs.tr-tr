---
title: ASP.NET Core Razor sayfasına yeni bir alan ekleyin
author: rick-anderson
description: Entity Framework Core ile bir Razor sayfası için yeni bir alan ekleme işlemi açıklanır
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: e280bc9553113982a1f1a77eabab32575c905237
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862297"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>ASP.NET Core Razor sayfasına yeni bir alan ekleyin

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

Bu bölümdeki [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:

* Modele yeni bir alan ekleyin.
* Yeni alan şema değişikliği veritabanına geçirin.

EF Code First otomatik olarak Code First bir veritabanı oluşturmak için kullanırken:

* Veritabanı şeması öğesinden oluşturulan model sınıfları ile eşitlenmiş olup olmadığını izlemek için veritabanında bir tablo ekler.
* EF, model sınıfları sahip bir veritabanı eşit değilse bir özel durum oluşturur.

Şema/modelinin eşitlenmiş otomatik doğrulama tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Film modeli derecelendirme özellik ekleme

Açık *Models/Movie.cs* dosya ve ekleme bir `Rating` özelliği:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Uygulama oluşturun.

Düzen *Pages/Movies/Index.cshtml*ve bir `Rating` alan:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

Aşağıdaki sayfalar güncelleştirin:

* Ekleme `Rating` silme ve ayrıntıları sayfaları alanı.
* Güncelleştirme [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) ile bir `Rating` alan.
* Ekleme `Rating` Düzenle sayfasında alanı.

Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz. Şimdi uygulamayı oluşturur çalıştırırsanız bir `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Bu hata, veritabanının film tablonun şeması farklı olan güncelleştirilmiş film model sınıfı kaynaklanır. (Yok hiçbir `Rating` veritabanı tablosundaki sütun.)

Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şeması kullanarak veritabanını yeniden oluşturma Entity Framework vardır. Bu yaklaşım, Geliştirme döngüsünün başlarında kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır. Olumsuz tarafı, veritabanındaki mevcut verileri kaybetmek ' dir. Bu yaklaşım, bir üretim veritabanında kullanmayın! Bir veritabanı üzerinde şema değişikliklerini bırakarak ve veritabanı test verileri ile otomatik olarak oluşturmak için bir Başlatıcısı kullanarak genellikle bir uygulama geliştirmek için bir üretken yoludur.

2. Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin. Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır. Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.

3. Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.

Bu öğreticide, Code First Migrations'ı kullanın.

Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını. Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie` blok.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

Bkz: [SeedData.cs dosya tamamlandı](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).

Çözümü oluşturun.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a>Derecelendirme alanı için bir geçiş ekleyin

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.
PMC'de aşağıdaki komutları girin:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Komutu framework bildirir:

* Karşılaştırma `Movie` ile model `Movie` DB şema.
* Yeni modeline DB şema geçişi için kod oluşturun.

Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır. Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.

<a name="ssox"></a>

DB tüm kayıtların silerseniz, başlatıcı DB çekirdeğini ve dahil `Rating` alan. Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX). SSOX veritabanını silmek için:

* Veritabanı içinde SSOX seçin.
* Veritabanını sağ tıklatın ve seçin *Sil*.
* Denetleme **var olan bağlantıları kapatın**.
* Seçin **Tamam**.
* İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştir:

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- copy/paste this tab to the next. Not worth an include  --> SQLite geçişleri desteklemez.

* Veritabanını silin veya veritabanı adını değiştirmek *appsettings.json* dosya.
* Silme *geçişler* klasörü (ve klasördeki tüm dosyaları).

Aşağıdaki .NET Core CLI komutları çalıştırın:

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

SQLite geçişleri desteklemez.

* Veritabanını silin veya veritabanı adını değiştirmek *appsettings.json* dosya.
* Silme *geçişler* klasörü (ve klasördeki tüm dosyaları).

Aşağıdaki .NET Core CLI komutları çalıştırın:

```console
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan. Veritabanı çekirdek değeri oluşturulmuş değil, bir kesme noktası ayarlayın `SeedData.Initialize` yöntemi.

> [!div class="step-by-step"]
> [Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: doğrulama ekleme](xref:tutorials/razor-pages/validation)
