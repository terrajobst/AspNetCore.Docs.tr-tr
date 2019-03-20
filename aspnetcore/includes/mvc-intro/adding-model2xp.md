---
ms.openlocfilehash: 4a569a3f7f804919b7fd6e481c2ec5a3acdfbbe8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265530"
---
<a name="cli"></a>

## <a name="perform-initial-migration"></a>İlk geçiş gerçekleştirme

Komut satırından aşağıdaki .NET Core CLI komutları çalıştırın:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`dotnet ef migrations add InitialCreate` Komut, ilk veritabanı şeması oluşturmak için kod oluşturur. Belirtilen model şeması dayanır `DbContext` (içinde *Models/MvcMovieContext.cs* dosyası). `Initial` Bağımsız değişkeni, geçişlerin adlandırmak için kullanılır. Herhangi bir adı kullanabilirsiniz, ancak bu kurala göre geçiş tanımlayan bir ad seçin. Bkz: [geçişler giriş](xref:data/ef-mvc/migrations#introduction-to-migrations) daha fazla bilgi için.

`dotnet ef database update` Komutu çalıştırmaları `Up` yöntemi *geçişleri /\<zaman damgası > _InitialCreate.cs* dosyasını veritabanı oluşturur.
