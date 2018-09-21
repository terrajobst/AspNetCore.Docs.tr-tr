---
title: ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: 722676b6765c32f3d11d5a3e23a5bea6ebe5488d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523265"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="4c0d6-103">ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="4c0d6-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="4c0d6-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="4c0d6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="4c0d6-105">Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="4c0d6-106">Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="4c0d6-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="4c0d6-107">[Yükleme yönergeleri](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="4c0d6-107">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="4c0d6-108">Eşzamanlılık çakışmaları</span><span class="sxs-lookup"><span data-stu-id="4c0d6-108">Concurrency conflicts</span></span>

<span data-ttu-id="4c0d6-109">Bir eşzamanlılık çakışması ortaya olduğunda:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="4c0d6-110">Bir kullanıcı bir varlığın düzenleme sayfasına götürür.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="4c0d6-111">İlk kullanıcının değişiklik DB'ye yazılmadan önce başka bir kullanıcı aynı varlık güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="4c0d6-112">Eşzamanlılık algılama etkin değil, eş zamanlı güncelleştirmeler olduğunda:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="4c0d6-113">Son güncelleştirme WINS.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-113">The last update wins.</span></span> <span data-ttu-id="4c0d6-114">Diğer bir deyişle, son güncelleştirme değerleri Veritabanına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="4c0d6-115">Geçerli güncelleştirme ilk kaybolur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="4c0d6-116">İyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="4c0d6-116">Optimistic concurrency</span></span>

<span data-ttu-id="4c0d6-117">İyimser eşzamanlılık eşzamanlılık çakışmalarını olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="4c0d6-118">Örneğin, Jane departmanı Düzen sayfasını ziyaret ve İngilizce departmanı için bütçe 350,000.00 $0,00 değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

<span data-ttu-id="4c0d6-120">Jane tıkladığında önce **Kaydet**, John aynı sayfayı ziyaret eder ve alanın başlangıç tarihi 1/9/2013 1/9/2007'deki değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date.png)

<span data-ttu-id="4c0d6-122">Jane tıkladığında **Kaydet** ilk ve her tarayıcı dizin sayfası görüntülendiğinde değiştirme görür.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

<span data-ttu-id="4c0d6-124">John tıkladığında **Kaydet** yine de bir bütçe $350,000.00 birini gösteren bir Düzenle sayfasında.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="4c0d6-125">Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="4c0d6-126">İyimser eşzamanlılık aşağıdaki seçenekleri içerir:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="4c0d6-127">Bir kullanıcı değiştirmiş hangi özelliğinin kaydını ve yalnızca ilgili sütunları DB'de güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="4c0d6-128">Bu senaryoda, veri kaybolacak.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="4c0d6-129">Farklı özellikler iki kullanıcı tarafından güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="4c0d6-130">Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="4c0d6-131">Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="4c0d6-132">Bu yaklaşım:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-132">This approach:</span></span>
 
  * <span data-ttu-id="4c0d6-133">Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-133">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="4c0d6-134">Genellikle bir web uygulaması pratik değildir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-134">Is generally not practical in a web app.</span></span> <span data-ttu-id="4c0d6-135">Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="4c0d6-136">Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-136">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="4c0d6-137">Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-137">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="4c0d6-138">Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-138">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="4c0d6-139">Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-139">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="4c0d6-140">Bu yaklaşım olarak adlandırılan bir *istemci WINS* veya *WINS'te son* senaryo.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="4c0d6-141">(Tüm istemci değerlerinden veri deposunda nedir üzerinde önceliklidir.) Eşzamanlılık işleme için kodlama yapmazsanız, istemci WINS otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="4c0d6-142">Can'ın değişiklik DB'de güncelleştirilmesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="4c0d6-143">Genellikle, bir uygulamayı atadığınız:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-143">Typically, the app would:</span></span>

  * <span data-ttu-id="4c0d6-144">Bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-144">Display an error message.</span></span>
  * <span data-ttu-id="4c0d6-145">Verilerin geçerli durumunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-145">Show the current state of the data.</span></span>
  * <span data-ttu-id="4c0d6-146">Değişiklikleri uygulamak verin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-146">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="4c0d6-147">Bu adlı bir *Store WINS* senaryo.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-147">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="4c0d6-148">(Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide Store WINS senaryo uygulamanız.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-148">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="4c0d6-149">Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-149">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="4c0d6-150">Eşzamanlılığı işleme</span><span class="sxs-lookup"><span data-stu-id="4c0d6-150">Handling concurrency</span></span> 

<span data-ttu-id="4c0d6-151">Ne zaman bir özellik olarak yapılandırıldığında bir [eşzamanlılık belirteci](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="4c0d6-151">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="4c0d6-152">EF Core getirildi sonra'nın özellik değiştirilmedi doğrular.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-152">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="4c0d6-153">Onay gerçekleşir, [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-153">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="4c0d6-154">Özelliği, getirildikten sonra değiştirilmişse, bir [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-154">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="4c0d6-155">DB ve veri modeli oluşturma destekleyecek şekilde yapılandırılması gerekir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-155">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="4c0d6-156">Bir özellik eşzamanlılık çakışmaları algılama</span><span class="sxs-lookup"><span data-stu-id="4c0d6-156">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="4c0d6-157">Eşzamanlılık çakışmaları ile özellik düzeyinde algılanamıyor [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-157">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="4c0d6-158">Öznitelik, birden çok modelin özellikleri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-158">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="4c0d6-159">Daha fazla bilgi için [veri ek açıklamaları-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="4c0d6-159">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="4c0d6-160">`[ConcurrencyCheck]` Özniteliği, bu öğreticide kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-160">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="4c0d6-161">Bir satırda eşzamanlılık çakışmaları algılama</span><span class="sxs-lookup"><span data-stu-id="4c0d6-161">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="4c0d6-162">Eşzamanlılık çakışmalarını algılamak için bir [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) sütun izleme modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-162">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="4c0d6-163">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="4c0d6-163">`rowversion` :</span></span>

* <span data-ttu-id="4c0d6-164">SQL Server özeldir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-164">Is SQL Server specific.</span></span> <span data-ttu-id="4c0d6-165">Diğer veritabanlarının benzer bir özellik sağlamayabilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-165">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="4c0d6-166">Veritabanından getirildikten sonra'nın bir varlık değiştirilmediğinden belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-166">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="4c0d6-167">Bir sıralı bir veritabanı oluşturur `rowversion` her zaman satır artan sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-167">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="4c0d6-168">İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi içeren getirilen değeri `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-168">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="4c0d6-169">Güncelleştirilen satır değiştiyse:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-169">If the row being updated has changed:</span></span>

 * <span data-ttu-id="4c0d6-170">`rowversion` getirilen değeri ile eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-170">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="4c0d6-171">`Update` Veya `Delete` olduğundan, komut satır Bul yok `Where` yan tümcesi içeren getirilen `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-171">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="4c0d6-172">A `DbUpdateConcurrencyException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-172">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="4c0d6-173">EF Core tarafından hiçbir satır güncelleştirildiğinde içinde bir `Update` veya `Delete` komutu, bir eşzamanlılık özel durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-173">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="4c0d6-174">Departman varlığa izleme özelliği ekleme</span><span class="sxs-lookup"><span data-stu-id="4c0d6-174">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="4c0d6-175">İçinde *Models/Department.cs*, RowVersion adlı izleme özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-175">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="4c0d6-176">[Zaman damgası](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği, bu sütunda yer belirtir `Where` yan tümcesi `Update` ve `Delete` komutları.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-176">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="4c0d6-177">Adlandırılan öznitelik `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` veri türü SQL önce `rowversion` türü değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-177">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="4c0d6-178">Fluent API'si izleme özelliği de belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-178">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="4c0d6-179">Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-179">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="4c0d6-180">Önceki kodun gösterdiği vurgulanmış `WHERE` yan tümcesi içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-180">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="4c0d6-181">Varsa DB `RowVersion` eşit değildir `RowVersion` parametre (`@p2`), satır güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-181">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="4c0d6-182">Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-182">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="4c0d6-183">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satır sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-183">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="4c0d6-184">Hayır, satır güncelleştirilir, EF Core oluşturur bir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-184">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="4c0d6-185">T-SQL EF Core oluşturur Visual Studio çıktı penceresinde görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-185">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="4c0d6-186">DB update</span><span class="sxs-lookup"><span data-stu-id="4c0d6-186">Update the DB</span></span>

<span data-ttu-id="4c0d6-187">Ekleme `RowVersion` geçiş gerektiren DB modeli özelliğini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-187">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="4c0d6-188">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-188">Build the project.</span></span> <span data-ttu-id="4c0d6-189">Bir komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-189">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="4c0d6-190">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-190">The preceding commands:</span></span>

* <span data-ttu-id="4c0d6-191">Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-191">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="4c0d6-192">Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-192">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="4c0d6-193">Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-193">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="4c0d6-194">DB güncelleştirilemedi geçişlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-194">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="4c0d6-195">İskele Departmanlar modeli</span><span class="sxs-lookup"><span data-stu-id="4c0d6-195">Scaffold the Departments model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4c0d6-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c0d6-196">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="4c0d6-197">Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-the-student-model) ve `Department` model sınıfı için.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-197">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Department` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4c0d6-198">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4c0d6-198">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="4c0d6-199">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-199">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

------

<span data-ttu-id="4c0d6-200">Önceki komut iskelesini kurar `Department` modeli.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-200">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="4c0d6-201">Projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-201">Open the project in Visual Studio.</span></span>

<span data-ttu-id="4c0d6-202">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-202">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="4c0d6-203">Departmanlar dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="4c0d6-203">Update the Departments Index page</span></span>

<span data-ttu-id="4c0d6-204">Oluşturulan yapı iskelesi altyapısı bir `RowVersion` sütunu için dizin sayfasını, ancak bu alanı olmamalıdır görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="4c0d6-205">Bu öğreticide, son baytı `RowVersion` eşzamanlılık anlamanıza yardımcı olması için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="4c0d6-206">Son bayta kalan benzersiz olması garanti yoktur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="4c0d6-207">Gerçek bir uygulamada görüntüleyemiyordu `RowVersion` veya son baytı `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="4c0d6-208">Dizin Sayfası güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-208">Update the Index page:</span></span>

* <span data-ttu-id="4c0d6-209">Dizin bölümlerine değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="4c0d6-210">Biçimlendirme içeren değiştirin `RowVersion` son baytı ile `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="4c0d6-211">FirstMidName tam adı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="4c0d6-212">Güncelleştirilen sayfaya aşağıdaki işaretlemeyi gösterir:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="4c0d6-213">Düzenleme sayfa modeli güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4c0d6-213">Update the Edit page model</span></span>

<span data-ttu-id="4c0d6-214">Güncelleştirme *pages\departments\edit.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="4c0d6-215">Bir eşzamanlılık algılanması [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) güncelleştirilmesi `rowVersion` alınan varlık değeri.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-215">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="4c0d6-216">EF Core özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="4c0d6-217">Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), `DbUpdateConcurrencyException` özel durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="4c0d6-218">Önceki kodda, `Department.RowVersion` varlık getirildi değeri olur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="4c0d6-219">`OriginalValue` DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="4c0d6-220">Aşağıdaki kod, istemci değerler (Bu yönteme gönderilen değerler) ve DB değerleri alır:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="4c0d6-221">DB sahip her bir sütunun ne deftere nakledilen farklı değerler için aşağıdaki kod bir özel hata iletisi ekler `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="4c0d6-222">Aşağıdaki vurgulanmış kodu kümeleri `RowVersion` değerden yeni değere Veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="4c0d6-223">Kullanıcının bir sonraki tıklayışında **Kaydet**, düzenleme sayfası son görüntülenmesini yakalandı beri gerçekleşen eşzamanlılık hataları.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="4c0d6-224">`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski olan `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="4c0d6-225">Razor Sayfası'nda `ModelState` değerinin ikisi de mevcut olduğunda alan üzerinde model özellik değerlerini öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="4c0d6-226">Güncelleştirme düzenleme sayfası</span><span class="sxs-lookup"><span data-stu-id="4c0d6-226">Update the Edit page</span></span>

<span data-ttu-id="4c0d6-227">Güncelleştirme *Pages/Departments/Edit.cshtml* aşağıdaki işaretlemeyle:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="4c0d6-228">Önceki işaretlemesi:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-228">The preceding markup:</span></span>

* <span data-ttu-id="4c0d6-229">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="4c0d6-230">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-230">Adds a hidden row version.</span></span> <span data-ttu-id="4c0d6-231">`RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="4c0d6-232">Son baytı görüntüler `RowVersion` hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="4c0d6-233">Değiştirir `ViewData` türü kesin belirlenmiş ile `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="4c0d6-234">Eşzamanlılık çakışmalarını düzenleme sayfası ile test</span><span class="sxs-lookup"><span data-stu-id="4c0d6-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="4c0d6-235">İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="4c0d6-236">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="4c0d6-237">Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="4c0d6-238">Birinci sekmede tıklayın **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="4c0d6-239">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="4c0d6-240">' A tıklayın ve ilk tarayıcı sekmesine adını değiştirmek **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-240">Change the name in the first browser tab and click **Save**.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="4c0d6-242">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="4c0d6-243">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="4c0d6-244">İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-244">Change a different field in the second browser tab.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="4c0d6-246">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-246">Click **Save**.</span></span> <span data-ttu-id="4c0d6-247">DB değerleri eşleşmeyen tüm alanlar için hata iletileri görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-247">You see error messages for all fields that don't match the DB values:</span></span>

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error.png)

<span data-ttu-id="4c0d6-249">Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="4c0d6-250">Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="4c0d6-251">Çıkış sekmesi. İstemci tarafı doğrulama hata iletisi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-251">Tab out. Client-side validation removes the error message.</span></span>

![Departman düzenleme sayfa hata iletisi](concurrency/_static/cv.png)

<span data-ttu-id="4c0d6-253">Tıklayın **Kaydet** yeniden.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-253">Click **Save** again.</span></span> <span data-ttu-id="4c0d6-254">İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="4c0d6-255">Dizin sayfasında kaydedilen değerler görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="4c0d6-256">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="4c0d6-256">Update the Delete page</span></span>

<span data-ttu-id="4c0d6-257">Delete sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="4c0d6-258">Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="4c0d6-259">`Department.RowVersion` Varlık getirildi satır sürümü andır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="4c0d6-260">EF Core SQL DELETE komutu oluşturduğunda, WHERE yan tümcesi ile içerdiği `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="4c0d6-261">Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="4c0d6-262">`RowVersion` SQL DELETE komutu eşleşmiyor `RowVersion` DB'de.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="4c0d6-263">DbUpdateConcurrencyException özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="4c0d6-264">`OnGetAsync` çağrılır `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="4c0d6-265">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="4c0d6-265">Update the Delete page</span></span>

<span data-ttu-id="4c0d6-266">Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="4c0d6-267">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="4c0d6-268">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="4c0d6-269">Bir hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-269">Adds an error message.</span></span>
* <span data-ttu-id="4c0d6-270">FullName FirstMidName değiştirir **yönetici** alan.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="4c0d6-271">Değişiklikleri `RowVersion` son bayt görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="4c0d6-272">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-272">Adds a hidden row version.</span></span> <span data-ttu-id="4c0d6-273">`RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="4c0d6-274">Eşzamanlılık çakışmalarını silme sayfası ile test</span><span class="sxs-lookup"><span data-stu-id="4c0d6-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="4c0d6-275">Test bölümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-275">Create a test department.</span></span>

<span data-ttu-id="4c0d6-276">İki tarayıcılar örneklerinde DELETE test departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="4c0d6-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="4c0d6-277">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="4c0d6-278">Sağ **Sil** seçin ve test departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="4c0d6-279">Tıklayın **Düzenle** köprü test bölümü için.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="4c0d6-280">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="4c0d6-281">İlk tarayıcı sekmesine bütçede değiştirip'ı **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="4c0d6-282">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="4c0d6-283">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="4c0d6-284">Test bölümü ikinci sekmesinden silin. Bir eşzamanlılık hatası DB geçerli değerlerle görüntüdür.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="4c0d6-285">Tıklayarak **Sil** sürece varlığını siler `RowVersion` updated.department silinmiş olmuştur.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="4c0d6-286">Bkz: [devralma](xref:data/ef-mvc/inheritance) nasıl bir veri modeli devralır.</span><span class="sxs-lookup"><span data-stu-id="4c0d6-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="4c0d6-287">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4c0d6-287">Additional resources</span></span>

* [<span data-ttu-id="4c0d6-288">EF Core eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="4c0d6-288">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="4c0d6-289">EF Core eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="4c0d6-289">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="4c0d6-290">Önceki</span><span class="sxs-lookup"><span data-stu-id="4c0d6-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
