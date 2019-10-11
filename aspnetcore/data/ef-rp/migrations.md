---
title: ASP.NET Core geçişleri ile EF Core Razor Pages-4/8
author: rick-anderson
description: Bu öğreticide, ASP.NET Core MVC uygulamasında veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliğini kullanmaya başlayabilirsiniz.
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/migrations
ms.openlocfilehash: e6f44a32a473f8f3bacfa4d9608deba2f879b6b6
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259713"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>ASP.NET Core geçişleri ile EF Core Razor Pages-4/8

, [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog)ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliği tanıtılmıştır.

Yeni bir uygulama geliştirildiğinde, veri modeli sıklıkla değişir. Modelin her değiştirilişinde, model veritabanıyla eşitlenmemiş olur. Bu öğretici serisi, mevcut değilse veritabanını oluşturmak için Entity Framework yapılandırılarak başlatılır. Veri modelinin her değiştirilişinde veritabanını bırakmalısınız. Uygulamanın bir sonraki çalıştırılışında, `EnsureCreated` ' a yapılan çağrı, veritabanını yeni veri modeliyle eşleşecek şekilde yeniden oluşturur. @No__t-0 sınıfı yeni veritabanını temel almak için çalışır.

Veritabanını veri modeliyle eşitlenmiş halde tutmaya yönelik bu yaklaşım, uygulamayı üretime dağıtana kadar iyi çalışır. Uygulama üretimde çalıştığında genellikle saklanması gereken verileri depolar. Uygulama her değişiklik yapıldığında (yeni sütun ekleme gibi) bir test veritabanıyla başlayamaz. EF Core geçişleri özelliği, yeni bir veritabanı oluşturmak yerine EF Core veritabanı şemasını güncelleştirmesine olanak sağlayarak bu sorunu çözer.

Veri modeli değiştiğinde veritabanını bırakıp yeniden oluşturmak yerine, geçişler şemayı güncelleştirir ve var olan verileri korur.

[!INCLUDE[](~/includes/sqlite-warn.md)]

## <a name="drop-the-database"></a>Veritabanını bırak

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Veritabanını silmek için **SQL Server Nesne Gezgini** (ssox) kullanın veya **Paket Yöneticisi konsolunda** (PMC) şu komutu çalıştırın:

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* EF CLı araçları 'nı yüklemek için komut isteminde aşağıdaki komutu çalıştırın:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  ```

* Komut isteminde proje klasörüne gidin. Proje klasörü *Contosouniversity. csproj* dosyasını içerir.

* *Cu. db* dosyasını silin veya şu komutu çalıştırın:

  ```dotnetcli
  dotnet ef database drop --force
  ```

---

## <a name="create-an-initial-migration"></a>İlk geçiş oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

PMC 'de şu komutları çalıştırın:

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Komut isteminin proje klasöründe olduğundan emin olun ve aşağıdaki komutları çalıştırın:

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

## <a name="up-and-down-methods"></a>Yukarı ve aşağı Yöntemler

EF Core `migrations add` komutu, veritabanını oluşturmak için kod üretti. Bu geçiş kodu *geçişleri @ no__t-1timestamp > _ınitialcreate. cs* dosyasında bulunur. @No__t-1 sınıfının `Up` yöntemi, veri modeli varlık kümelerine karşılık gelen veritabanı tablolarını oluşturur. @No__t-0 yöntemi, aşağıdaki örnekte gösterildiği gibi bunları siler:

[!code-csharp[](intro/samples/cu30/Migrations/20190731193522_InitialCreate.cs)]

Önceki kod ilk geçişe yöneliktir. Kod:

* @No__t-0 komutu tarafından oluşturuldu. 
* @No__t-0 komutu tarafından yürütülür.
* Veritabanı bağlamı sınıfı tarafından belirtilen veri modeli için bir veritabanı oluşturur.

Dosya adı için geçiş adı parametresi (örnekteki "ınitialcreate") kullanılır. Geçiş adı herhangi bir geçerli dosya adı olabilir. Geçiş sırasında nelerin yapıldığını özetleyen bir sözcük veya tümcecik seçmek en iyisidir. Örneğin, bir departman tablosu ekleyen bir geçişe "AddDepartmentTable" adı verilir.

## <a name="the-migrations-history-table"></a>Geçişler geçmiş tablosu

* Veritabanını incelemek için SSOX veya SQLite aracınızı kullanın.
* @No__t-0 tablosuna eklendiğine dikkat edin. @No__t-0 tablosu, veritabanına hangi geçişlerin uygulandığını izler.
* @No__t-0 tablosundaki verileri görüntüleyin. İlk geçiş için bir satır gösterir.

## <a name="the-data-model-snapshot"></a>Veri modeli anlık görüntüsü

Geçişler, *geçiş/SchoolContextModelSnapshot. cs*içindeki geçerli veri modelinin *anlık görüntüsünü* oluşturur. Bir geçiş eklediğinizde, EF geçerli veri modelini Snapshot dosyası ile karşılaştırarak nelerin değiştirildiğini belirler.

Anlık görüntü dosyası veri modelinin durumunu izlediğinden, `<timestamp>_<migrationname>.cs` dosyasını silerek bir geçişi silemezsiniz. En son geçişi geri yüklemek için `migrations remove` komutunu kullanmanız gerekir. Bu komut, geçişi siler ve anlık görüntünün doğru şekilde sıfırlanmasını sağlar. Daha fazla bilgi için bkz. [DotNet EF geçişleri kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

## <a name="remove-ensurecreated"></a>Yeniden oluşturulmasını kaldır

Bu öğretici serisi `EnsureCreated` kullanılarak başlatıldı. `EnsureCreated` bir geçişler geçmişi tablosu oluşturmaz ve geçişle birlikte kullanılamaz. Bu, veritabanının düşürülme ve sıklıkla yeniden oluşturulduğu test veya hızlı prototip oluşturma için tasarlanmıştır.

Bu noktadan sonra öğreticiler, geçişleri kullanacaktır.

*Data/Dbınınitializer. cs*dosyasında aşağıdaki satırı açıklama olarak inceleyin:

```csharp
context.Database.EnsureCreated();
```
Uygulamayı çalıştırın ve veritabanının çalıştığını doğrulayın.

## <a name="applying-migrations-in-production"></a>Üretimde geçişleri uygulama

Uygulama başlangıcında, üretim uygulamalarının [Database. Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) **olarak çağırmalarını** öneririz. `Migrate` bir sunucu grubuna dağıtılan bir uygulamadan çağrılmamalıdır. Uygulama birden çok sunucu örneğine ölçekleniyorsa, veritabanı şeması güncelleştirmelerinin birden çok sunucudan oluşmaması veya okuma/yazma erişimiyle çakışmamasını sağlamak zordur.

Veritabanı geçişi, dağıtımın bir parçası olarak ve denetimli bir şekilde yapılmalıdır. Üretim veritabanı geçiş yaklaşımları şunları içerir:

* SQL betikleri oluşturmak ve dağıtımda SQL betikleri kullanmak için geçişleri kullanma.
* Denetlenen bir ortamdan `dotnet ef database update` çalıştırılıyor.

## <a name="troubleshooting"></a>Sorun giderme

Uygulama SQL Server LocalDB kullanıyorsa ve aşağıdaki özel durumu görüntülüyorsa:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Çözüm, bir komut isteminde `dotnet ef database update` ' i çalıştırmak olabilir.

### <a name="additional-resources"></a>Ek kaynaklar

* [Clı EF Core](/ef/core/miscellaneous/cli/dotnet).
* [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticide, veri modeli, varlık özellikleri ve yeni varlıklar eklenerek oluşturulur.

> [!div class="step-by-step"]
> [Önceki öğretici](xref:data/ef-rp/sort-filter-page)
> [sonraki öğretici](xref:data/ef-rp/complex-data-model)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu öğreticide, veri modeli değişikliklerini yönetmek için EF Core geçişleri özelliği kullanılır.

Çözemediğiniz sorunlarla karşılaşırsanız, [Tamamlanmış uygulamayı](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)indirin.

Yeni bir uygulama geliştirildiğinde, veri modeli sıklıkla değişir. Modelin her değiştirilişinde, model veritabanıyla eşitlenmemiş olur. Bu öğretici, mevcut değilse veritabanını oluşturmak için Entity Framework yapılandırılarak başlatılır. Veri modelinin her değiştirilişinde:

* DB bırakılır.
* EF, modelle eşleşen yeni bir tane oluşturur.
* Uygulama, DB 'yi test verileriyle birlikte oluşturur.

Bu yaklaşım, VERITABANıNı veri modeliyle eşitlenmiş halde tutmak, uygulamayı üretime dağıtana kadar iyi çalışır. Uygulama üretimde çalıştığında genellikle saklanması gereken verileri depolar. Uygulama her değişiklik yapıldığında (yeni sütun ekleme gibi) bir test DB ile başlayamaz. EF Core geçişleri özelliği, yeni bir VERITABANı oluşturmak yerine EF Core DB şemasını güncelleştirmesine olanak sağlayarak bu sorunu çözer.

Veri modeli değiştiğinde VERITABANıNı bırakıp yeniden oluşturmak yerine, geçişler şemayı güncelleştirir ve mevcut verileri korur.

## <a name="drop-the-database"></a>Veritabanını bırak

**SQL Server Nesne Gezgini** (ssox) veya `database drop` komutunu kullanın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**Paket Yöneticisi konsolunda** (PMC), aşağıdaki komutu çalıştırın:

```PMC
Drop-Database
```

Yardım bilgileri almak için PMC 'ten `Get-Help about_EntityFrameworkCore` ' i çalıştırın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Bir komut penceresi açın ve proje klasörüne gidin. Proje klasörü *Startup.cs* dosyasını içerir.

Komut penceresine şunu girin:

 ```dotnetcli
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a>İlk geçiş oluşturma ve VERITABANıNı güncelleştirme

Projeyi derleyin ve ilk geçişi oluşturun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a>Yukarı ve aşağı yöntemlerini inceleyin

EF Core `migrations add` komutu, DB oluşturmak için kod oluşturdu. Bu geçiş kodu *geçişleri @ no__t-1timestamp > _ınitialcreate. cs* dosyasında bulunur. @No__t-1 sınıfının `Up` yöntemi, veri modeli varlık kümelerine karşılık gelen VERITABANı tablolarını oluşturur. @No__t-0 yöntemi, aşağıdaki örnekte gösterildiği gibi bunları siler:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Geçişler, bir geçiş için veri modeli değişikliklerini uygulamak üzere `Up` yöntemini çağırır. Güncelleştirmeyi geri almak için bir komut girdiğinizde, geçişler `Down` yöntemini çağırır.

Önceki kod ilk geçişe yöneliktir. Bu kod, `migrations add InitialCreate` komutu çalıştırıldığında oluşturulmuştur. Dosya adı için geçiş adı parametresi (örnekteki "ınitialcreate") kullanılır. Geçiş adı herhangi bir geçerli dosya adı olabilir. Geçiş sırasında nelerin yapıldığını özetleyen bir sözcük veya tümcecik seçmek en iyisidir. Örneğin, bir departman tablosu ekleyen bir geçişe "AddDepartmentTable" adı verilir.

İlk geçiş oluşturulur ve VERITABANı varsa:

* DB oluşturma kodu oluşturulur.
* DB, veri modeliyle zaten eşleştiğinden, DB oluşturma kodunun çalıştırılması gerekmiyor. DB oluşturma kodu çalıştırılsa, VERITABANı veri modeliyle zaten eşleştiğinden hiçbir değişiklik yapmaz.

Uygulama yeni bir ortama dağıtıldığında, DB oluşturmak için DB oluşturma kodunun çalıştırılması gerekir.

Daha önce VERITABANı bırakılmıştı ve mevcut olmadığından geçişler yeni DB 'yi oluşturur.

### <a name="the-data-model-snapshot"></a>Veri modeli anlık görüntüsü

Geçişler *geçişlerde/SchoolContextModelSnapshot. cs*' de geçerli veritabanı şemasının *anlık görüntüsünü* oluşturur. Bir geçiş eklediğinizde EF, veri modeli Snapshot dosyası ile karşılaştırılarak nelerin değiştirildiğini belirler.

Bir geçişi silmek için aşağıdaki komutu kullanın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Geçişi Kaldır

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations remove
```

Daha fazla bilgi için bkz. [DotNet EF geçişleri kaldır](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

---

Geçişleri Kaldır komutu geçişi siler ve anlık görüntünün doğru şekilde sıfırlanmasını sağlar.

### <a name="remove-ensurecreated-and-test-the-app"></a>Uygulamayı kaldırın ve uygulamayı test edin

Erken geliştirme için `EnsureCreated` kullanıldı. Bu öğreticide geçişler kullanılır. `EnsureCreated` aşağıdaki sınırlamalara sahiptir:

* Geçişleri atlar ve DB ve şema oluşturur.
* Geçişler tablosu oluşturmaz.
* Geçişlerle *kullanılamaz.*
* , DB 'nin bıraktığı ve sıklıkla yeniden oluşturulduğu test veya hızlı prototipleme için tasarlanmıştır.

@No__t kaldır-0:

```csharp
context.Database.EnsureCreated();
```

Uygulamayı çalıştırın ve DB 'nin çalıştığını doğrulayın.

### <a name="inspect-the-database"></a>Veritabanını inceleyin

DB 'yi denetlemek için **SQL Server Nesne Gezgini** kullanın. @No__t-0 tablosuna eklendiğine dikkat edin. @No__t-0 tablosu, hangi geçişlerin VERITABANıNA uygulandığını izler. @No__t-0 tablosundaki verileri görüntüleyin, ilk geçiş için bir satır gösterir. Önceki CLı çıkış örneğinde yer alan son oturum, bu satırı oluşturan INSERT ifadesini gösterir.

Uygulamayı çalıştırın ve her şeyin çalıştığını doğrulayın.

## <a name="applying-migrations-in-production"></a>Üretimde geçişleri uygulama

Uygulama başlangıcında, üretim uygulamalarının [Database. Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) **çağrısını yapmanızı** öneririz. `Migrate` sunucu grubundaki bir uygulamadan çağrılmamalıdır. Örneğin, uygulama bulutu genişleme ile dağıtılmışsa (uygulamanın birden çok örneği çalışır).

Veritabanı geçişi, dağıtımın bir parçası olarak ve denetimli bir şekilde yapılmalıdır. Üretim veritabanı geçiş yaklaşımları şunları içerir:

* SQL betikleri oluşturmak ve dağıtımda SQL betikleri kullanmak için geçişleri kullanma.
* Denetlenen bir ortamdan `dotnet ef database update` çalıştırılıyor.

EF Core, herhangi bir geçişin çalıştırılması gerektiğini görmek için `__MigrationsHistory` tablosunu kullanır. DB güncel değilse, hiçbir geçiş çalıştırılmaz.

## <a name="troubleshooting"></a>Sorun giderme

[Tamamlanmış uygulamayı](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21snapshots/cu-part4-migrations)indirin.

Uygulama aşağıdaki özel durumu oluşturur:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Çözüm: Run `dotnet ef database update`

### <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).
* [Paket Yöneticisi Konsolu (Visual Studio)](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> [Önceki](xref:data/ef-rp/sort-filter-page)
> [İleri](xref:data/ef-rp/complex-data-model)

::: moniker-end

