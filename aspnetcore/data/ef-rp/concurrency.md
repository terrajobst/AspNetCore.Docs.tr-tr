---
title: ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: ff9e52df63f9c9f47ee659a68beb28b773a114a1
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202698"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="3036e-103">ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="3036e-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="3036e-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="3036e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="3036e-105">Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3036e-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="3036e-106">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="3036e-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="3036e-107">Eşzamanlılık çakışmaları</span><span class="sxs-lookup"><span data-stu-id="3036e-107">Concurrency conflicts</span></span>

<span data-ttu-id="3036e-108">Bir eşzamanlılık çakışması ortaya olduğunda:</span><span class="sxs-lookup"><span data-stu-id="3036e-108">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="3036e-109">Bir kullanıcı bir varlığın düzenleme sayfasına götürür.</span><span class="sxs-lookup"><span data-stu-id="3036e-109">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="3036e-110">İlk kullanıcının değişiklik DB'ye yazılmadan önce başka bir kullanıcı aynı varlık güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="3036e-110">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="3036e-111">Eşzamanlılık algılama etkin değil, eş zamanlı güncelleştirmeler olduğunda:</span><span class="sxs-lookup"><span data-stu-id="3036e-111">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="3036e-112">Son güncelleştirme WINS.</span><span class="sxs-lookup"><span data-stu-id="3036e-112">The last update wins.</span></span> <span data-ttu-id="3036e-113">Diğer bir deyişle, son güncelleştirme değerleri Veritabanına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-113">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="3036e-114">Geçerli güncelleştirme ilk kaybolur.</span><span class="sxs-lookup"><span data-stu-id="3036e-114">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="3036e-115">İyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="3036e-115">Optimistic concurrency</span></span>

<span data-ttu-id="3036e-116">İyimser eşzamanlılık eşzamanlılık çakışmalarını olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın.</span><span class="sxs-lookup"><span data-stu-id="3036e-116">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="3036e-117">Örneğin, Jane departmanı Düzen sayfasını ziyaret ve İngilizce departmanı için bütçe 350,000.00 $0,00 değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3036e-117">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

<span data-ttu-id="3036e-119">Jane tıkladığında önce **Kaydet**, John aynı sayfayı ziyaret eder ve alanın başlangıç tarihi 1/9/2013 1/9/2007'deki değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3036e-119">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date.png)

<span data-ttu-id="3036e-121">Jane tıkladığında **Kaydet** ilk ve her tarayıcı dizin sayfası görüntülendiğinde değiştirme görür.</span><span class="sxs-lookup"><span data-stu-id="3036e-121">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

<span data-ttu-id="3036e-123">John tıkladığında **Kaydet** yine de bir bütçe $350,000.00 birini gösteren bir Düzenle sayfasında.</span><span class="sxs-lookup"><span data-stu-id="3036e-123">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="3036e-124">Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="3036e-124">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="3036e-125">İyimser eşzamanlılık aşağıdaki seçenekleri içerir:</span><span class="sxs-lookup"><span data-stu-id="3036e-125">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="3036e-126">Bir kullanıcı değiştirmiş hangi özelliğinin kaydını ve yalnızca ilgili sütunları DB'de güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="3036e-126">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="3036e-127">Bu senaryoda, veri kaybolacak.</span><span class="sxs-lookup"><span data-stu-id="3036e-127">In the scenario, no data would be lost.</span></span> <span data-ttu-id="3036e-128">Farklı özellikler iki kullanıcı tarafından güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="3036e-128">Different properties were updated by the two users.</span></span> <span data-ttu-id="3036e-129">Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler.</span><span class="sxs-lookup"><span data-stu-id="3036e-129">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="3036e-130">Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3036e-130">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="3036e-131">Bu yaklaşım:</span><span class="sxs-lookup"><span data-stu-id="3036e-131">This approach:</span></span>
 
  * <span data-ttu-id="3036e-132">Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.</span><span class="sxs-lookup"><span data-stu-id="3036e-132">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="3036e-133">Genellikle bir web uygulaması pratik değildir.</span><span class="sxs-lookup"><span data-stu-id="3036e-133">Is generally not practical in a web app.</span></span> <span data-ttu-id="3036e-134">Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="3036e-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="3036e-135">Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-135">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="3036e-136">Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3036e-136">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="3036e-137">Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3036e-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="3036e-138">Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri.</span><span class="sxs-lookup"><span data-stu-id="3036e-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="3036e-139">Bu yaklaşım olarak adlandırılan bir *istemci WINS* veya *WINS'te son* senaryo.</span><span class="sxs-lookup"><span data-stu-id="3036e-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="3036e-140">(Tüm istemci değerlerinden veri deposunda nedir üzerinde önceliklidir.) Eşzamanlılık işleme için kodlama yapmazsanız, istemci WINS otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="3036e-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="3036e-141">Can'ın değişiklik DB'de güncelleştirilmesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="3036e-142">Genellikle, bir uygulamayı atadığınız:</span><span class="sxs-lookup"><span data-stu-id="3036e-142">Typically, the app would:</span></span>

  * <span data-ttu-id="3036e-143">Bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3036e-143">Display an error message.</span></span>
  * <span data-ttu-id="3036e-144">Verilerin geçerli durumunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3036e-144">Show the current state of the data.</span></span>
  * <span data-ttu-id="3036e-145">Değişiklikleri uygulamak verin.</span><span class="sxs-lookup"><span data-stu-id="3036e-145">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="3036e-146">Bu adlı bir *Store WINS* senaryo.</span><span class="sxs-lookup"><span data-stu-id="3036e-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="3036e-147">(Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide Store WINS senaryo uygulamanız.</span><span class="sxs-lookup"><span data-stu-id="3036e-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="3036e-148">Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="3036e-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="3036e-149">Eşzamanlılığı işleme</span><span class="sxs-lookup"><span data-stu-id="3036e-149">Handling concurrency</span></span> 

<span data-ttu-id="3036e-150">Ne zaman bir özellik olarak yapılandırıldığında bir [eşzamanlılık belirteci](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="3036e-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="3036e-151">EF Core getirildi sonra'nın özellik değiştirilmedi doğrular.</span><span class="sxs-lookup"><span data-stu-id="3036e-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="3036e-152">Onay gerçekleşir, [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3036e-152">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="3036e-153">Özelliği, getirildikten sonra değiştirilmişse, bir [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3036e-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="3036e-154">DB ve veri modeli oluşturma destekleyecek şekilde yapılandırılması gerekir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="3036e-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="3036e-155">Bir özellik eşzamanlılık çakışmaları algılama</span><span class="sxs-lookup"><span data-stu-id="3036e-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="3036e-156">Eşzamanlılık çakışmaları ile özellik düzeyinde algılanamıyor [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="3036e-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="3036e-157">Öznitelik, birden çok modelin özellikleri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="3036e-158">Daha fazla bilgi için [veri ek açıklamaları-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="3036e-158">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="3036e-159">`[ConcurrencyCheck]` Özniteliği, bu öğreticide kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="3036e-159">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="3036e-160">Bir satırda eşzamanlılık çakışmaları algılama</span><span class="sxs-lookup"><span data-stu-id="3036e-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="3036e-161">Eşzamanlılık çakışmalarını algılamak için bir [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) sütun izleme modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="3036e-161">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="3036e-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="3036e-162">`rowversion` :</span></span>

* <span data-ttu-id="3036e-163">SQL Server özeldir.</span><span class="sxs-lookup"><span data-stu-id="3036e-163">Is SQL Server specific.</span></span> <span data-ttu-id="3036e-164">Diğer veritabanlarının benzer bir özellik sağlamayabilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="3036e-165">Veritabanından getirildikten sonra'nın bir varlık değiştirilmediğinden belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3036e-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="3036e-166">Bir sıralı bir veritabanı oluşturur `rowversion` her zaman satır artan sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="3036e-167">İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi içeren getirilen değeri `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="3036e-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="3036e-168">Güncelleştirilen satır değiştiyse:</span><span class="sxs-lookup"><span data-stu-id="3036e-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="3036e-169">`rowversion` getirilen değeri ile eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="3036e-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="3036e-170">`Update` Veya `Delete` olduğundan, komut satır Bul yok `Where` yan tümcesi içeren getirilen `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="3036e-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="3036e-171">A `DbUpdateConcurrencyException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3036e-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="3036e-172">EF Core tarafından hiçbir satır güncelleştirildiğinde içinde bir `Update` veya `Delete` komutu, bir eşzamanlılık özel durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3036e-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="3036e-173">Departman varlığa izleme özelliği ekleme</span><span class="sxs-lookup"><span data-stu-id="3036e-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="3036e-174">İçinde *Models/Department.cs*, RowVersion adlı izleme özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3036e-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="3036e-175">[Zaman damgası](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği, bu sütunda yer belirtir `Where` yan tümcesi `Update` ve `Delete` komutları.</span><span class="sxs-lookup"><span data-stu-id="3036e-175">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="3036e-176">Adlandırılan öznitelik `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` veri türü SQL önce `rowversion` türü değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="3036e-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="3036e-177">Fluent API'si izleme özelliği de belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="3036e-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="3036e-178">Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="3036e-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="3036e-179">Önceki kodun gösterdiği vurgulanmış `WHERE` yan tümcesi içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3036e-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="3036e-180">Varsa DB `RowVersion` eşit değildir `RowVersion` parametre (`@p2`), satır güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="3036e-181">Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:</span><span class="sxs-lookup"><span data-stu-id="3036e-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="3036e-182">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satır sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="3036e-182">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="3036e-183">Hayır, satır güncelleştirilir, EF Core oluşturur bir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="3036e-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="3036e-184">T-SQL EF Core oluşturur Visual Studio çıktı penceresinde görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3036e-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="3036e-185">DB update</span><span class="sxs-lookup"><span data-stu-id="3036e-185">Update the DB</span></span>

<span data-ttu-id="3036e-186">Ekleme `RowVersion` geçiş gerektiren DB modeli özelliğini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3036e-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="3036e-187">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3036e-187">Build the project.</span></span> <span data-ttu-id="3036e-188">Bir komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="3036e-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="3036e-189">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="3036e-189">The preceding commands:</span></span>

* <span data-ttu-id="3036e-190">Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.</span><span class="sxs-lookup"><span data-stu-id="3036e-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="3036e-191">Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="3036e-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="3036e-192">Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3036e-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="3036e-193">DB güncelleştirilemedi geçişlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="3036e-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="3036e-194">İskele Departmanlar modeli</span><span class="sxs-lookup"><span data-stu-id="3036e-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="3036e-195">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="3036e-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="3036e-196">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="3036e-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3036e-197">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3036e-197">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

<span data-ttu-id="3036e-198">Önceki komut iskelesini kurar `Department` modeli.</span><span class="sxs-lookup"><span data-stu-id="3036e-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="3036e-199">Projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="3036e-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="3036e-200">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3036e-200">Build the project.</span></span> <span data-ttu-id="3036e-201">Yapı aşağıdaki gibi hatalar oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3036e-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="3036e-202">Genel olarak değiştirme `_context.Department` için `_context.Departments` ("s" eklemek diğer bir deyişle, `Department`).</span><span class="sxs-lookup"><span data-stu-id="3036e-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="3036e-203">7 oluşum bulundu ve güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="3036e-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="3036e-204">Departmanlar dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="3036e-204">Update the Departments Index page</span></span>

<span data-ttu-id="3036e-205">Oluşturulan yapı iskelesi altyapısı bir `RowVersion` sütunu için dizin sayfasını, ancak bu alanı olmamalıdır görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="3036e-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="3036e-206">Bu öğreticide, son baytı `RowVersion` eşzamanlılık anlamanıza yardımcı olması için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3036e-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="3036e-207">Son bayta kalan benzersiz olması garanti yoktur.</span><span class="sxs-lookup"><span data-stu-id="3036e-207">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="3036e-208">Gerçek bir uygulamada görüntüleyemiyordu `RowVersion` veya son baytı `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3036e-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="3036e-209">Dizin Sayfası güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="3036e-209">Update the Index page:</span></span>

* <span data-ttu-id="3036e-210">Dizin bölümlerine değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3036e-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="3036e-211">Biçimlendirme içeren değiştirin `RowVersion` son baytı ile `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3036e-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="3036e-212">FirstMidName tam adı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3036e-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="3036e-213">Güncelleştirilen sayfaya aşağıdaki işaretlemeyi gösterir:</span><span class="sxs-lookup"><span data-stu-id="3036e-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="3036e-214">Düzenleme sayfa modeli güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="3036e-214">Update the Edit page model</span></span>

<span data-ttu-id="3036e-215">Güncelleştirme *pages\departments\edit.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="3036e-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="3036e-216">Bir eşzamanlılık algılanması [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) güncelleştirilmesi `rowVersion` alınan varlık değeri.</span><span class="sxs-lookup"><span data-stu-id="3036e-216">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="3036e-217">EF Core özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="3036e-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="3036e-218">Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), `DbUpdateConcurrencyException` özel durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3036e-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="3036e-219">Önceki kodda, `Department.RowVersion` varlık getirildi değeri olur.</span><span class="sxs-lookup"><span data-stu-id="3036e-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="3036e-220">`OriginalValue` DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="3036e-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="3036e-221">Aşağıdaki kod, istemci değerler (Bu yönteme gönderilen değerler) ve DB değerleri alır:</span><span class="sxs-lookup"><span data-stu-id="3036e-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="3036e-222">DB sahip her bir sütunun ne deftere nakledilen farklı değerler için aşağıdaki kod bir özel hata iletisi ekler `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="3036e-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="3036e-223">Aşağıdaki vurgulanmış kodu kümeleri `RowVersion` değerden yeni değere Veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="3036e-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="3036e-224">Kullanıcının bir sonraki tıklayışında **Kaydet**, düzenleme sayfası son görüntülenmesini yakalandı beri gerçekleşen eşzamanlılık hataları.</span><span class="sxs-lookup"><span data-stu-id="3036e-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="3036e-225">`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski olan `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="3036e-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="3036e-226">Razor Sayfası'nda `ModelState` değerinin ikisi de mevcut olduğunda alan üzerinde model özellik değerlerini öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="3036e-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="3036e-227">Güncelleştirme düzenleme sayfası</span><span class="sxs-lookup"><span data-stu-id="3036e-227">Update the Edit page</span></span>

<span data-ttu-id="3036e-228">Güncelleştirme *Pages/Departments/Edit.cshtml* aşağıdaki işaretlemeyle:</span><span class="sxs-lookup"><span data-stu-id="3036e-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="3036e-229">Önceki işaretlemesi:</span><span class="sxs-lookup"><span data-stu-id="3036e-229">The preceding markup:</span></span>

* <span data-ttu-id="3036e-230">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3036e-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="3036e-231">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="3036e-231">Adds a hidden row version.</span></span> <span data-ttu-id="3036e-232">`RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3036e-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="3036e-233">Son baytı görüntüler `RowVersion` hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="3036e-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="3036e-234">Değiştirir `ViewData` türü kesin belirlenmiş ile `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="3036e-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="3036e-235">Eşzamanlılık çakışmalarını düzenleme sayfası ile test</span><span class="sxs-lookup"><span data-stu-id="3036e-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="3036e-236">İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="3036e-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="3036e-237">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="3036e-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="3036e-238">Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="3036e-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="3036e-239">Birinci sekmede tıklayın **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="3036e-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="3036e-240">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="3036e-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="3036e-241">' A tıklayın ve ilk tarayıcı sekmesine adını değiştirmek **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="3036e-241">Change the name in the first browser tab and click **Save**.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="3036e-243">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="3036e-244">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3036e-244">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="3036e-245">İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3036e-245">Change a different field in the second browser tab.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="3036e-247">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3036e-247">Click **Save**.</span></span> <span data-ttu-id="3036e-248">DB değerleri eşleşmeyen tüm alanlar için hata iletileri görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="3036e-248">You see error messages for all fields that don't match the DB values:</span></span>

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error.png)

<span data-ttu-id="3036e-250">Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz.</span><span class="sxs-lookup"><span data-stu-id="3036e-250">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="3036e-251">Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="3036e-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="3036e-252">Çıkış sekmesi. İstemci tarafı doğrulama hata iletisi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="3036e-252">Tab out. Client-side validation removes the error message.</span></span>

![Departman düzenleme sayfa hata iletisi](concurrency/_static/cv.png)

<span data-ttu-id="3036e-254">Tıklayın **Kaydet** yeniden.</span><span class="sxs-lookup"><span data-stu-id="3036e-254">Click **Save** again.</span></span> <span data-ttu-id="3036e-255">İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="3036e-256">Dizin sayfasında kaydedilen değerler görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="3036e-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="3036e-257">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="3036e-257">Update the Delete page</span></span>

<span data-ttu-id="3036e-258">Delete sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="3036e-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="3036e-259">Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar.</span><span class="sxs-lookup"><span data-stu-id="3036e-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="3036e-260">`Department.RowVersion` Varlık getirildi satır sürümü andır.</span><span class="sxs-lookup"><span data-stu-id="3036e-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="3036e-261">EF Core SQL DELETE komutu oluşturduğunda, WHERE yan tümcesi ile içerdiği `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3036e-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="3036e-262">Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:</span><span class="sxs-lookup"><span data-stu-id="3036e-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="3036e-263">`RowVersion` SQL DELETE komutu eşleşmiyor `RowVersion` DB'de.</span><span class="sxs-lookup"><span data-stu-id="3036e-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="3036e-264">DbUpdateConcurrencyException özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3036e-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="3036e-265">`OnGetAsync` çağrılır `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="3036e-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="3036e-266">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="3036e-266">Update the Delete page</span></span>

<span data-ttu-id="3036e-267">Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="3036e-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="3036e-268">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="3036e-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="3036e-269">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3036e-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="3036e-270">Bir hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="3036e-270">Adds an error message.</span></span>
* <span data-ttu-id="3036e-271">FullName FirstMidName değiştirir **yönetici** alan.</span><span class="sxs-lookup"><span data-stu-id="3036e-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="3036e-272">Değişiklikleri `RowVersion` son bayt görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="3036e-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="3036e-273">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="3036e-273">Adds a hidden row version.</span></span> <span data-ttu-id="3036e-274">`RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3036e-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="3036e-275">Eşzamanlılık çakışmalarını silme sayfası ile test</span><span class="sxs-lookup"><span data-stu-id="3036e-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="3036e-276">Test bölümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3036e-276">Create a test department.</span></span>

<span data-ttu-id="3036e-277">İki tarayıcılar örneklerinde DELETE test departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="3036e-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="3036e-278">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="3036e-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="3036e-279">Sağ **Sil** seçin ve test departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="3036e-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="3036e-280">Tıklayın **Düzenle** köprü test bölümü için.</span><span class="sxs-lookup"><span data-stu-id="3036e-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="3036e-281">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="3036e-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="3036e-282">İlk tarayıcı sekmesine bütçede değiştirip'ı **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="3036e-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="3036e-283">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="3036e-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="3036e-284">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3036e-284">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="3036e-285">Test bölümü ikinci sekmesinden silin. Bir eşzamanlılık hatası DB geçerli değerlerle görüntüdür.</span><span class="sxs-lookup"><span data-stu-id="3036e-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="3036e-286">Tıklayarak **Sil** sürece varlığını siler `RowVersion` updated.department silinmiş olmuştur.</span><span class="sxs-lookup"><span data-stu-id="3036e-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="3036e-287">Bkz: [devralma](xref:data/ef-mvc/inheritance) nasıl bir veri modeli devralır.</span><span class="sxs-lookup"><span data-stu-id="3036e-287">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="3036e-288">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3036e-288">Additional resources</span></span>

* [<span data-ttu-id="3036e-289">EF Core eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="3036e-289">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="3036e-290">EF Core eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="3036e-290">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="3036e-291">Önceki</span><span class="sxs-lookup"><span data-stu-id="3036e-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
