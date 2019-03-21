---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320298"
---
<span data-ttu-id="e9d2b-101">Oluşturulan kimlik veritabanı kod gerektirir [Entity Framework Code Migrations](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="e9d2b-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="e9d2b-102">Bir geçiş oluşturmak ve veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="e9d2b-102">Create a migration and update the database.</span></span> <span data-ttu-id="e9d2b-103">Örneğin, aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e9d2b-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e9d2b-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9d2b-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e9d2b-105">Visual Studio **Paket Yöneticisi Konsolu**:</span><span class="sxs-lookup"><span data-stu-id="e9d2b-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e9d2b-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e9d2b-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="e9d2b-107">"CreateIdentitySchema" name parametresi için `Add-Migration` rastgele komutu.</span><span class="sxs-lookup"><span data-stu-id="e9d2b-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="e9d2b-108">`"CreateIdentitySchema"` geçiş açıklamaktadır.</span><span class="sxs-lookup"><span data-stu-id="e9d2b-108">`"CreateIdentitySchema"` describes the migration.</span></span>
