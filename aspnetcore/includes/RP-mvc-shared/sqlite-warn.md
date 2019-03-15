---
ms.openlocfilehash: 1f8d3913c83aaf5fe6ec2cec482a30f0f066c16b
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841699"
---

> [!NOTE]
> <span data-ttu-id="ca271-101">Bu öğretici için Entity Framework Core kullanan *geçişler* mümkün olduğunca özelliği.</span><span class="sxs-lookup"><span data-stu-id="ca271-101">For this tutorial you use the Entity Framework Core *migrations* feature where possible.</span></span> <span data-ttu-id="ca271-102">Geçişler, veri modelindeki değişikliklerle eşleştirmek için veritabanı şemasını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="ca271-102">Migrations updates the database schema to match changes in the data model.</span></span> <span data-ttu-id="ca271-103">Ancak, geçişler yalnızca tür EF Core sağlayıcının desteklediği değişiklikleri yapabilirsiniz ve SQLite sağlayıcısının özellikleri sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="ca271-103">However, migrations can only do the kinds of changes that the EF Core provider supports, and the SQLite provider's capabilities are limited.</span></span> <span data-ttu-id="ca271-104">Örneğin, sütun ekleme desteklenir, ancak kaldırmayı veya sütunu değiştirmek desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="ca271-104">For example, adding a column is supported, but removing or changing a column is not supported.</span></span> <span data-ttu-id="ca271-105">Bir sütunu değiştirme veya kaldırmak için bir geçiş oluşturduysanız `ef migrations add` komut başarılı ancak `ef database update` komutu başarısız oluyor.</span><span class="sxs-lookup"><span data-stu-id="ca271-105">If a migration is created to remove or change a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="ca271-106">Bu sınırlamaları nedeniyle, Bu öğretici için SQLite şema değişiklikleri geçişleri kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="ca271-106">Due to these limitations, this tutorial doesn't use migrations for SQLite schema changes.</span></span> <span data-ttu-id="ca271-107">Bunun yerine, şema değiştiğinde, bırak ve veritabanını yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ca271-107">Instead, when the schema changes, you drop and re-create the database.</span></span>
>
><span data-ttu-id="ca271-108">SQLite sınırlamaları için geçici bir tablo yeniden oluşturma şeyler olduğunda tablo değişiklikleri gerçekleştirmek için geçişler kod el ile yazmaktır.</span><span class="sxs-lookup"><span data-stu-id="ca271-108">The workaround for the SQLite limitations is to manually write migrations code to perform a table rebuild when something in the table changes.</span></span> <span data-ttu-id="ca271-109">Bir tablo yeniden oluşturma içerir:</span><span class="sxs-lookup"><span data-stu-id="ca271-109">A table rebuild involves:</span></span>
>
>* <span data-ttu-id="ca271-110">Varolan bir tabloyu yeniden adlandırma.</span><span class="sxs-lookup"><span data-stu-id="ca271-110">Renaming the existing table.</span></span>
>* <span data-ttu-id="ca271-111">Yeni bir tablo oluşturuluyor.</span><span class="sxs-lookup"><span data-stu-id="ca271-111">Creating a new table.</span></span>
>* <span data-ttu-id="ca271-112">Verileri yeni tabloya eski tablodan kopyalama.</span><span class="sxs-lookup"><span data-stu-id="ca271-112">Copying data from the old table to the new table.</span></span>
>* <span data-ttu-id="ca271-113">Eski tablosu bırakılıyor.</span><span class="sxs-lookup"><span data-stu-id="ca271-113">Dropping the old table.</span></span>
>
><span data-ttu-id="ca271-114">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="ca271-114">For more information, see the following resources:</span></span>
>
> * [<span data-ttu-id="ca271-115">SQLite EF Core veritabanı sağlayıcısı sınırlamaları</span><span class="sxs-lookup"><span data-stu-id="ca271-115">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="ca271-116">Geçiş kodu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ca271-116">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="ca271-117">Veri çekirdeği oluşturma</span><span class="sxs-lookup"><span data-stu-id="ca271-117">Data seeding</span></span>](/ef/core/modeling/data-seeding)