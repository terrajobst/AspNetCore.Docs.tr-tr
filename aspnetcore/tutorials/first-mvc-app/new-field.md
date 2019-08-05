---
title: ASP.NET Core MVC uygulamasına yeni bir alan ekleme
author: rick-anderson
description: Bir modele yeni bir alan eklemek ve bu değişikliği veritabanına geçirmek için Entity Framework Code First Migrations kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: f22a90dd7662bfb34e2f7adb87fa611012d78533
ms.sourcegitcommit: b5e63714afc26e94be49a92619586df5189ed93a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739591"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC uygulamasına yeni bir alan ekleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde [Entity Framework](/ef/core/get-started/aspnetcore/new-db) için Code First Migrations kullanılır:

* Modele yeni bir alan ekleyin.
* Yeni alanı veritabanına geçirin.

EF Code First otomatik olarak bir veritabanı oluşturmak için kullanıldığında, Code First:

* Veritabanının şemasını izlemek için veritabanına tablo ekler.
* Veritabanının oluşturulduğu model sınıflarıyla eşitlenmiş olduğunu doğrular. Bunlar eşitlenmiş değilse EF bir özel durum oluşturur. Bu, tutarsız veritabanı/kod sorunlarını bulmayı kolaylaştırır.

## <a name="add-a-rating-property-to-the-movie-model"></a>Film modeline bir derecelendirme özelliği ekleyin

`Rating` *Modeller/film. cs*'ye bir özellik ekleyin:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Uygulama oluşturma

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

 Ctrl+Shift+B

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

`dotnet build`

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Komut ⌘ + B

------

`Movie` Sınıfa yeni bir alan eklediyseniz, bu yeni özelliğin dahil edilmesini sağlamak için bağlama beyaz listesini güncelleştirmeniz gerekir. *MoviesController.cs*içinde, `[Bind]` özelliğidahil`Rating` etmek için hem hem `Create` de `Edit` eylem yöntemlerinin özniteliğini güncelleştirin:

```csharp
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Yeni `Rating` özelliği tarayıcı görünümünde görüntülemek, oluşturmak ve düzenlemek için görünüm şablonlarını güncelleştirin.

*/Views/movies/Index.cshtml* dosyasını düzenleyin ve bir `Rating` alan ekleyin:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

*/Views/movies/Create.cshtml* ile bir `Rating` alanı güncelleştirin.

# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[Visual Studio/Mac için Visual Studio](#tab/visual-studio+visual-studio-mac)

Önceki "form grubunu kopyalayabilir/yapıştırabilir" ve IntelliSense 'in alanları güncelleştirmenize yardımcı olmasına izin verebilirsiniz. IntelliSense, [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ile birlikte çalışmaktadır.

![Geliştirici, görünümün ikinci Label öğesinde için ASP-for öznitelik değeri için R harfini yazmıştır. Bir IntelliSense bağlamsal menüsü, listede otomatik olarak vurgulanmış olan derecelendirme dahil olmak üzere kullanılabilir alanları gösterir. Geliştirici alana tıkladığında veya klavyede ENTER tuşuna bastığında, değer derecelendirme olarak ayarlanır.](new-field/_static/cr.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- This tab intentionally left blank. -->

---

Kalan şablonları güncelleştirin.

`SeedData` Sınıfını yeni sütun için bir değer sağlayacak şekilde güncelleştirin. Aşağıda örnek bir değişiklik gösterilmektedir, ancak her biri `new Movie`için bu değişikliği yapmak isteyeceksiniz.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

VERITABANı yeni alanı içerecek şekilde güncelleştirilene kadar uygulama çalışmaz. Şimdi çalıştırıldığında, şunlar `SqlException` oluşur:

`SqlException: Invalid column name 'Rating'.`

Bu hata, güncelleştirilmiş film modeli sınıfı varolan veritabanının film tablosunun şemasından farklı olduğu için oluşur. (Veritabanı tablosunda sütun `Rating` yok.)

Hatayı çözmek için birkaç yaklaşım vardır:

1. Entity Framework yeni model sınıfı şemasına göre otomatik olarak veritabanını bırakıp yeniden oluşturmayı sağlayabilirsiniz. Bu yaklaşım, bir test veritabanı üzerinde etkin geliştirme yaparken geliştirme döngüsünün başlarında çok daha kolay. modeli ve veritabanı şemasını birlikte hızla gelişmenize olanak tanır. Bunun yanında, bu yaklaşımı bir üretim veritabanında kullanmak istemezsiniz, ancak bu, veritabanında var olan verileri kaybetmeniz olur. Bir veritabanının test verileriyle otomatik olarak çekirdeği oluşturmak için bir başlatıcı kullanılması, genellikle bir uygulama geliştirmenin üretken bir yoludur. Bu, erken geliştirme ve SQLite kullanılırken iyi bir yaklaşımdır.

2. Mevcut veritabanının şemasını model sınıflarıyla eşleşecek şekilde açıkça değiştirin. Bu yaklaşımın avantajı, verilerinizi tutmanızı kullanmaktır. Bu değişikliği el ile ya da bir veritabanı değişiklik betiği oluşturarak yapabilirsiniz.

3. Veritabanı şemasını güncelleştirmek için Code First Migrations kullanın.

Bu öğretici için Code First Migrations kullanılır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**Araçlar** menüsünde **NuGet Paket Yöneticisi > Paket Yöneticisi konsolu**' nu seçin.

  ![PMC menüsü](adding-model/_static/pmc.png)

PMC'de aşağıdaki komutları girin:

```powershell
Add-Migration Rating
Update-Database
```

Bu komut, geçiş çerçevesinin geçerli `Movie` DB şemasıyla geçerli `Movie` modeli incelemesini ve veritabanını yeni modele geçirmek için gerekli kodu oluşturmasını söyler. `Add-Migration`

"Derecelendirme" adı rastgele olur ve geçiş dosyasını adlandırmak için kullanılır. Geçiş dosyası için anlamlı bir ad kullanılması yararlı olur.

Veritabanındaki tüm kayıtlar silinirse, Initialize yöntemi veritabanını temel alır ve `Rating` alanını içerir.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Veritabanını silin ve geçişleri kullanarak veritabanını yeniden oluşturun. Veritabanını silmek için veritabanı dosyasını (*Mvcmovie. db*) silin. Ardından şu `ef database update` komutu çalıştırın:

```console
dotnet ef database update
```

---
<!-- End of VS tabs -->

Uygulamayı çalıştırın ve bir `Rating` alan ile film oluşturabileceğiniz/düzenleyebileceğiniz/görüntüleydiğinizi doğrulayın. `Rating` Alanı `Edit`, ,ve`Delete` şablonları görüntülemek için eklemeniz gerekir. `Details`

> [!div class="step-by-step"]
> [Önceki](search.md)İleri
> [](validation.md)
