---
title: ASP.NET Core Razor sayfasına yeni bir alan ekleyin
author: rick-anderson
description: Entity Framework Core ile bir Razor sayfası için yeni bir alan ekleme işlemi açıklanır
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: f471e4bd12510b1de78f3281dcb21d73975d0cb8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264734"
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

`Update-Database` Komutu veritabanı için şema değişiklikleri uygulamak için framework bildirir.

<a name="ssox"></a>

DB tüm kayıtların silerseniz, başlatıcı DB çekirdeğini ve dahil `Rating` alan. Tarayıcıda veya gelen silme bağlantıları kullanarak bunu yapabilirsiniz [Sql Server Nesne Gezgini](xref:tutorials/razor-pages/sql#ssox) (SSOX).

Başka bir seçenek veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanmaktır. SSOX veritabanında silmek için:

* Veritabanı içinde SSOX seçin.
* Veritabanını sağ tıklatın ve seçin *Sil*.
* Denetleme **var olan bağlantıları kapatın**.
* **Tamam**’ı seçin.
* İçinde [PMC](xref:tutorials/razor-pages/new-field#pmc), veritabanını güncelleştir:

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a>Bırakın ve veritabanını yeniden oluşturun

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanın. Veritabanını silmek için veritabanı dosyasını silin (*MvcMovie.db*). Ardından çalıştırın `ef database update` komutu:

```console
dotnet ef database update
```

---

Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan. Veritabanı çekirdek değeri oluşturulmuş değil, bir kesme noktası ayarlayın `SeedData.Initialize` yöntemi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticide YouTube sürümü](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> [Önceki: Arama ekleme](xref:tutorials/razor-pages/search)
> [sonraki: Doğrulama ekleme](xref:tutorials/razor-pages/validation)
