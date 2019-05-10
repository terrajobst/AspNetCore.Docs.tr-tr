---
title: Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin
author: rick-anderson
description: Bir model için yeni bir alan ekleyin ve bu değişiklik veritabanına geçirmek için Entity Framework Code First Migrations'ı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: cf8bb67703b564a711105123117498c94ab44e68
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902075"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için yeni bir alan ekleyin

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümdeki [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:

* Modele yeni bir alan ekleyin.
* Yeni alan veritabanına geçirin.

EF Code First için otomatik olarak kullanıldığında, Code First bir veritabanı oluşturun:

* Bir tablo veritabanı şeması izlemek için veritabanına ekler.
* Veritabanı öğesinden oluşturulan model sınıfları ile eşitlenmiş olduğunu doğrulayın. EF, bunlar eşit değilse bir özel durum oluşturur. Bu, tutarsız veritabanı kod sorunlarını bulmayı kolaylaştırır.

## <a name="add-a-rating-property-to-the-movie-model"></a>Film modeli için bir derecelendirme özelliği Ekle

Ekleme bir `Rating` özelliğini *Models/Movie.cs*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

(Ctrl + Shift + B) uygulaması oluşturun.

Yeni bir alan eklediğiniz çünkü `Movie` sınıfı için gereksinim duyduğunuz bağlama beyaz liste bu yeni özellik dahil olacak şekilde güncelleştirin. İçinde *MoviesController.cs*, güncelleştirme `[Bind]` özniteliği hem de `Create` ve `Edit` dahil etmek için eylem yöntemleri `Rating` özelliği:

```csharp
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Görünüm şablonları görüntülemek, oluşturmak ve yeni düzenlemek için güncelleştirme `Rating` Tarayıcı Görünümü özelliği.

Düzen */Views/Movies/Index.cshtml* dosya ve ekleme bir `Rating` alan:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

Güncelleştirme */Views/Movies/Create.cshtml* ile bir `Rating` alan.

# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[Visual Studio / Visual Studio Mac için](#tab/visual-studio+visual-studio-mac)

Önceki "form grubu" kopyala/yapıştır ve alanları güncelleştirin IntelliSense Yardım sağlar. IntelliSense ile birlikte çalışır [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro).

![Öznitelik değeri ASP R harfiyle Geliştirici yazdığını-için görünümün ikinci etiket öğesinde. IntelliSense bağlam menüsü listesinde otomatik olarak vurgulanır derecelendirme dahil olmak üzere kullanılabilir alanları gösteren görüntülendi. Geliştirici alana tıkladığında veya klavyedeki Enter tuşuna bastığında için derecelendirme değeri ayarlanır.](new-field/_static/cr.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- This tab intentionally left blank. -->

---

Güncelleştirme `SeedData` böylece yeni bir sütun için bir değer sağlar sınıfını. Aşağıda bir örnek değişikliği gösterilmiştir, ancak her biri için bu değişikliği yapmak istersiniz `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Uygulama DB yeni alanı içerecek şekilde güncelleştirilene kadar çalışmaz. Şimdi aşağıdaki çalıştırıldıysa `SqlException` oluşturulur:

`SqlException: Invalid column name 'Rating'.`

Güncelleştirilmiş film model sınıfı var olan veritabanının film tablo şemasından farklı olduğundan, bu hata oluşur. (Yok hiçbir `Rating` veritabanı tablosundaki sütun.)

Hatayı çözümlemek için birkaç yaklaşım vardır:

1. Otomatik olarak bırakın ve yeni model sınıfı şemasını temel alan veritabanını yeniden oluşturma Entity Framework vardır. Bu yaklaşım bir test veritabanında etkin geliştirme işi yaparken zaman Geliştirme döngüsünün başlarında çok kullanışlıdır; model ve veritabanı şeması birlikte hızla geliştirilebilen olanak tanır. Olumsuz tarafı, yine de veritabanında var olan veri kaybı olan — bir üretim veritabanında bu yaklaşımı kullanmak istemediğiniz şekilde! Bir başlatıcı bir veritabanı test verileri ile otomatik olarak oluşturmak için genellikle bir uygulama geliştirmek için üretken bir şekilde kullanmaktır. İyi bir yaklaşım erken geliştirmeye yönelik ve SQLite kullanırken budur.

2. Açıkça model sınıfları eşleşecek şekilde var olan veritabanı şeması değiştirin. Bu yaklaşımın avantajı, verilerinizi korumak olmasıdır. Bu değişikliği yapmak ya da el ile veya bir veritabanı oluşturma betiği değiştirin.

3. Veritabanı şemasını güncelleştirmek için Code First Migrations'ı kullanın.

Bu öğreticide, Code First Migrations kullanılır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**.

  ![PMC menüsü](adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` Komut Framework'e geçerli incelemek için geçiş `Movie` geçerli model `Movie` DB şema ve DB yeni modeline geçirme için gereken kodu oluşturun.

Adı "Sıralama" isteğe bağlıdır ve geçiş dosyasını adlandırmak için kullanılır. Geçiş dosya için anlamlı bir ad kullanmak yararlıdır.

Veritabanındaki tüm kayıtları silinen, Initialize yöntemi DB çekirdeğini ve dahil `Rating` alan.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Veritabanını silin ve veritabanını yeniden oluşturmaya geçişleri kullanın. Veritabanını silmek için veritabanı dosyasını silin (*MvcMovie.db*). Ardından çalıştırın `ef database update` komutu:

```console
dotnet ef database update
```

---
<!-- End of VS tabs -->

Uygulamayı çalıştırın ve kontrol edebilirsiniz oluşturma/düzenleme/görüntüleme filmlerle bir `Rating` alan. Eklemeniz gerekir `Rating` alanı `Edit`, `Details`, ve `Delete` şablonlarını görüntüleyin.

> [!div class="step-by-step"]
> [Önceki](search.md)
> [İleri](validation.md)
