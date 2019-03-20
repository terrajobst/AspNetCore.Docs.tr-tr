---
ms.openlocfilehash: 4a569a3f7f804919b7fd6e481c2ec5a3acdfbbe8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265530"
---
<a name="cli"></a>

## <a name="perform-initial-migration"></a><span data-ttu-id="4619c-101">İlk geçiş gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="4619c-101">Perform initial migration</span></span>

<span data-ttu-id="4619c-102">Komut satırından aşağıdaki .NET Core CLI komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4619c-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="4619c-103">`dotnet ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4619c-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="4619c-104">Belirtilen model şeması dayanır `DbContext` (içinde *Models/MvcMovieContext.cs* dosyası).</span><span class="sxs-lookup"><span data-stu-id="4619c-104">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="4619c-105">`Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4619c-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="4619c-106">Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin.</span><span class="sxs-lookup"><span data-stu-id="4619c-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="4619c-107">Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="4619c-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="4619c-108">`dotnet ef database update` Komutu çalıştırmaları `Up` yöntemi *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyasını veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4619c-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
