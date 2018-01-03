---
title: "Razor sayfalarının EF temel - eşzamanlılık - 8 8"
author: rick-anderson
description: "Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir."
keywords: "ASP.NET Core, Entity Framework Çekirdek eşzamanlılık"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 8862c6b9a5eb7ac3b6889071e4ce9ff6f02512c9
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
<span data-ttu-id="e0221-104">en-us /</span><span class="sxs-lookup"><span data-stu-id="e0221-104">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="e0221-105">Eşzamanlılık çakışmalarını - EF çekirdek Razor sayfaları (8 8) ile işleme</span><span class="sxs-lookup"><span data-stu-id="e0221-105">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="e0221-106">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [zel Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="e0221-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="e0221-107">Bu öğretici, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e0221-107">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="e0221-108">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="e0221-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="e0221-109">Eşzamanlılık çakışmaları</span><span class="sxs-lookup"><span data-stu-id="e0221-109">Concurrency conflicts</span></span>

<span data-ttu-id="e0221-110">Bir eşzamanlılık çakışması ortaya çıkar zaman:</span><span class="sxs-lookup"><span data-stu-id="e0221-110">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="e0221-111">Bir kullanıcıyı bir varlığın düzenleme sayfasına götürür.</span><span class="sxs-lookup"><span data-stu-id="e0221-111">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="e0221-112">İlk kullanıcının değişiklik DB yazılmadan önce başka bir kullanıcı aynı varlık güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e0221-112">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="e0221-113">Eşzamanlılık algılama etkin değilse, eş zamanlı güncelleştirmeler olduğunda:</span><span class="sxs-lookup"><span data-stu-id="e0221-113">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="e0221-114">Son güncelleştirme WINS.</span><span class="sxs-lookup"><span data-stu-id="e0221-114">The last update wins.</span></span> <span data-ttu-id="e0221-115">Diğer bir deyişle, son güncelleştirme değerleri Veritabanına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-115">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="e0221-116">Geçerli güncelleştirmeleri ilk kaybolur.</span><span class="sxs-lookup"><span data-stu-id="e0221-116">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="e0221-117">İyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="e0221-117">Optimistic concurrency</span></span>

<span data-ttu-id="e0221-118">İyimser eşzamanlılık eşzamanlılık çakışması olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın.</span><span class="sxs-lookup"><span data-stu-id="e0221-118">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="e0221-119">Örneğin, Jane departmanı Düzen sayfasını ziyaret ve bütçe İngilizce departmanı için 350,000.00 0,00 için değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e0221-119">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

<span data-ttu-id="e0221-121">Jane tıklar önce **kaydetmek**, John aynı sayfasını ziyaret ve başlangıç tarihi alanı 9/1/2007'den 9/1/2013'e değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e0221-121">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Başlangıç tarihi 2013 için değiştirme](concurrency/_static/change-date.png)

<span data-ttu-id="e0221-123">Jane tıklar **kaydetmek** ilk ve her tarayıcı dizin sayfası görüntülendiğinde değiştirmek görür.</span><span class="sxs-lookup"><span data-stu-id="e0221-123">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

<span data-ttu-id="e0221-125">John tıklar **kaydetmek** bir düzenleme sayfasında $350,000.00 bütçe görüntülenmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="e0221-125">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="e0221-126">Ne olacağını eşzamanlılık çakışmaları nasıl işleneceğini tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="e0221-126">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="e0221-127">İyimser eşzamanlılık aşağıdaki seçenekleri içerir:</span><span class="sxs-lookup"><span data-stu-id="e0221-127">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="e0221-128">Bir kullanıcı değiştirdi hangi özelliğinin izlemek ve yalnızca karşılık gelen sütunlara DB'de güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="e0221-128">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="e0221-129">Senaryoda, hiçbir veri kaybı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e0221-129">In the scenario, no data would be lost.</span></span> <span data-ttu-id="e0221-130">Farklı özellikler iki kullanıcı tarafından güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="e0221-130">Different properties were updated by the two users.</span></span> <span data-ttu-id="e0221-131">Birisi İngilizce departmanı gözatar başlatıldığında bunlar Jane'nın ve Can'ın değişiklikleri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e0221-131">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="e0221-132">Güncelleştirme bu yöntem, veri kaybına sebep çakışmaları sayısını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0221-132">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="e0221-133">Bu yaklaşım: * aynı özelliğe rakip bir değişiklik yaptıysanız veri kaybını önlemek olamaz.</span><span class="sxs-lookup"><span data-stu-id="e0221-133">This approach: * Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="e0221-134">* Olan bir web uygulamasında pratik genellikle.</span><span class="sxs-lookup"><span data-stu-id="e0221-134">* Is generally not practical in a web app.</span></span> <span data-ttu-id="e0221-135">Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="e0221-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="e0221-136">Büyük miktarlarda durumu koruma uygulama performansını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-136">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="e0221-137">* Bir varlık eşzamanlılık algılamayı karşılaştırılan uygulama karmaşıklığını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-137">* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="e0221-138">Jane'nın değişiklik üzerine Can'ın değişiklik izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0221-138">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="e0221-139">Sonraki birisi İngilizce departmanı gözatar, 1/9/2013 görürsünüz ve getirilen $350,000.00 değeri.</span><span class="sxs-lookup"><span data-stu-id="e0221-139">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="e0221-140">Bu yaklaşım adlı bir *istemci WINS* veya *WINS'de son* senaryo.</span><span class="sxs-lookup"><span data-stu-id="e0221-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="e0221-141">(İstemci tüm değerleri veri deposunda nedir üzerinden önceliklidir.) Hiçbir eşzamanlılık işleme için kodlama yapmazsanız, istemci WINS otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="e0221-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="e0221-142">DB güncelleştirilmiş Can'ın değişiklik engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="e0221-143">Uygulama zamanki: * bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e0221-143">Typically, the app would: * Display an error message.</span></span>
        <span data-ttu-id="e0221-144">* Verilerin geçerli durumunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="e0221-144">* Show the current state of the data.</span></span>
        <span data-ttu-id="e0221-145">* Değişiklikleri uygulamak verin.</span><span class="sxs-lookup"><span data-stu-id="e0221-145">* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="e0221-146">Bu adlı bir *deposu WINS* senaryo.</span><span class="sxs-lookup"><span data-stu-id="e0221-146">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="e0221-147">(Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide deposu WINS senaryo uygulayın.</span><span class="sxs-lookup"><span data-stu-id="e0221-147">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="e0221-148">Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı üzerine yazılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="e0221-148">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="e0221-149">Eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="e0221-149">Handling concurrency</span></span> 

<span data-ttu-id="e0221-150">Olarak bir özellik yapılandırıldığında bir [eşzamanlılık belirteci](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="e0221-150">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="e0221-151">EF çekirdek getirildi sonra'nın özellik değiştirilmemiş doğrular.</span><span class="sxs-lookup"><span data-stu-id="e0221-151">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="e0221-152">Onay oluşur zaman [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="e0221-152">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="e0221-153">Bunu getirildikten sonra özelliği değiştirildiğinde, bir [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) atılır.</span><span class="sxs-lookup"><span data-stu-id="e0221-153">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="e0221-154">Veritabanını ve veri modeli atma desteklemek için yapılandırılması gerekir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="e0221-154">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="e0221-155">Bir özelliğe eşzamanlılık çakışmalarını algılama</span><span class="sxs-lookup"><span data-stu-id="e0221-155">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="e0221-156">Eşzamanlılık çakışması algılanan özelliği düzeyindeki [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e0221-156">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="e0221-157">Öznitelik, model üzerinde birden çok özellikleri uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-157">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="e0221-158">Daha fazla bilgi için bkz: [veri ek açıklamaları-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="e0221-158">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="e0221-159">`[ConcurrencyCheck]` Özniteliği, bu öğreticide kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="e0221-159">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="e0221-160">Bir satırda eşzamanlılık çakışmalarını algılama</span><span class="sxs-lookup"><span data-stu-id="e0221-160">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="e0221-161">Eşzamanlılık çakışması algılamak için bir [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) sütun izleme modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="e0221-161">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="e0221-162">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="e0221-162">`rowversion` :</span></span>

* <span data-ttu-id="e0221-163">SQL Server özeldir.</span><span class="sxs-lookup"><span data-stu-id="e0221-163">Is SQL Server specific.</span></span> <span data-ttu-id="e0221-164">Diğer veritabanlarını benzer bir özellik sağlamayabilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-164">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="e0221-165">DB'den getirildikten sonra'nın bir varlık değiştirilmedi belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e0221-165">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="e0221-166">DB bir sıralı oluşturur `rowversion` her satırın artar numarası güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-166">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="e0221-167">İçinde bir `Update` veya `Delete` komutu, `Where` yan tümcesi içeren getirilen değeri `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="e0221-167">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="e0221-168">Güncelleştirilen satır değiştiyse:</span><span class="sxs-lookup"><span data-stu-id="e0221-168">If the row being updated has changed:</span></span>

 * <span data-ttu-id="e0221-169">`rowversion`getirilen değerle eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="e0221-169">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="e0221-170">`Update` Veya `Delete` olduğundan, komutları bir satır bulmak yok `Where` yan tümcesi içeren getirilen `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="e0221-170">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="e0221-171">A `DbUpdateConcurrencyException` atılır.</span><span class="sxs-lookup"><span data-stu-id="e0221-171">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="e0221-172">EF tarafından hiçbir satır güncelleştirildiğinde çekirdek içinde bir `Update` veya `Delete` komutu, bir eşzamanlılık özel durumu oluşur.</span><span class="sxs-lookup"><span data-stu-id="e0221-172">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="e0221-173">Departman varlığa izleme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="e0221-173">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="e0221-174">İçinde *Models/Department.cs*, RowVersion adlı bir izleme özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e0221-174">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="e0221-175">[Zaman damgası](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği belirtir. Bu sütunda yer `Where` yan tümcesi `Update` ve `Delete` komutları.</span><span class="sxs-lookup"><span data-stu-id="e0221-175">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="e0221-176">Öznitelik adı verilen `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` önce SQL veri türü `rowversion` türü değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="e0221-176">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="e0221-177">Fluent API izleme özelliği de belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e0221-177">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="e0221-178">Aşağıdaki kod bir bölüm adı güncelleştirildiğinde EF çekirdek tarafından oluşturulan T-SQL bölümünü gösterir:</span><span class="sxs-lookup"><span data-stu-id="e0221-178">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="e0221-179">Vurgulanmış kodu gösterir önceki `WHERE` yan tümcesi içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="e0221-179">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="e0221-180">Varsa DB `RowVersion` eşit olmayan `RowVersion` parametre (`@p2`), satır güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="e0221-180">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="e0221-181">Aşağıdaki vurgulanmış kodu tam olarak bir satır güncelleştirildi doğrular T-SQL gösterir:</span><span class="sxs-lookup"><span data-stu-id="e0221-181">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="e0221-182">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satırların sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="e0221-182">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="e0221-183">Hayır, satırları güncelleştirilir, EF çekirdek oluşturur bir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="e0221-183">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="e0221-184">Visual Studio çıktı penceresinde T-SQL EF çekirdeği oluşturur görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0221-184">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="e0221-185">DB güncelleştir</span><span class="sxs-lookup"><span data-stu-id="e0221-185">Update the DB</span></span>

<span data-ttu-id="e0221-186">Ekleme `RowVersion` özelliğini bir geçiş gerektirir DB modeli değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e0221-186">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="e0221-187">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e0221-187">Build the project.</span></span> <span data-ttu-id="e0221-188">Bir komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="e0221-188">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="e0221-189">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="e0221-189">The preceding commands:</span></span>

* <span data-ttu-id="e0221-190">Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.</span><span class="sxs-lookup"><span data-stu-id="e0221-190">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="e0221-191">Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="e0221-191">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="e0221-192">Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e0221-192">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="e0221-193">DB güncelleştirmek için geçiş çalışır.</span><span class="sxs-lookup"><span data-stu-id="e0221-193">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="e0221-194">İskele Departmanlar modeli</span><span class="sxs-lookup"><span data-stu-id="e0221-194">Scaffold the Departments model</span></span>

* <span data-ttu-id="e0221-195">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="e0221-195">Exit Visual Studio.</span></span>
* <span data-ttu-id="e0221-196">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="e0221-196">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e0221-197">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e0221-197">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="e0221-198">Yukarıdaki komut iskelesini kurar `Department` modeli.</span><span class="sxs-lookup"><span data-stu-id="e0221-198">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="e0221-199">Projesini Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="e0221-199">Open the project in Visual Studio.</span></span>

<span data-ttu-id="e0221-200">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e0221-200">Build the project.</span></span> <span data-ttu-id="e0221-201">Derleme hataları aşağıdaki gibi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="e0221-201">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="e0221-202">Genel olarak değiştirmek `_context.Department` için `_context.Departments` ("s" eklemek diğer bir deyişle, `Department`).</span><span class="sxs-lookup"><span data-stu-id="e0221-202">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="e0221-203">7 oluşumu bulundu ve güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="e0221-203">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="e0221-204">Güncelleştirme Departmanlar dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="e0221-204">Update the Departments Index page</span></span>

<span data-ttu-id="e0221-205">Oluşturulan iskele altyapısı bir `RowVersion` dizin sayfası, ancak bu alan için sütun döndürmemelidir görüntülenmesi.</span><span class="sxs-lookup"><span data-stu-id="e0221-205">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="e0221-206">Bu öğreticide son baytını `RowVersion` eşzamanlılık anlamanıza yardımcı olması için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e0221-206">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="e0221-207">Son bayta kalan benzersiz olması garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="e0221-207">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="e0221-208">Gerçek bir uygulama görüntüle olmayacaktır `RowVersion` veya son baytını `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="e0221-208">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="e0221-209">Dizin Sayfası güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="e0221-209">Update the Index page:</span></span>

* <span data-ttu-id="e0221-210">Dizin Departmanlar ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e0221-210">Replace Index with Departments.</span></span>
* <span data-ttu-id="e0221-211">Biçimlendirme içeren Değiştir `RowVersion` son baytını ile `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="e0221-211">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="e0221-212">FirstMidName FullName ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e0221-212">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="e0221-213">Aşağıdaki biçimlendirmede güncelleştirilmiş sayfası gösterir:</span><span class="sxs-lookup"><span data-stu-id="e0221-213">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="e0221-214">Güncelleştirmeyi Düzenle sayfası modeli</span><span class="sxs-lookup"><span data-stu-id="e0221-214">Update the Edit page model</span></span>

<span data-ttu-id="e0221-215">Güncelleştirme *pages\departments\edit.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e0221-215">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="e0221-216">Bir eşzamanlılık sorunu algılamak için [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) ile güncelleştirilmiş `rowVersion` alınan varlık değeri.</span><span class="sxs-lookup"><span data-stu-id="e0221-216">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="e0221-217">EF çekirdek özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="e0221-217">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="e0221-218">Hiçbir satır güncelleştirme komutu tarafından etkilenen varsa (özgün hiçbir satır sahip `RowVersion` değeri), bir `DbUpdateConcurrencyException` özel durumu oluşur.</span><span class="sxs-lookup"><span data-stu-id="e0221-218">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="e0221-219">Önceki kod `Department.RowVersion` varlık getirildi değeri olur.</span><span class="sxs-lookup"><span data-stu-id="e0221-219">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="e0221-220">`OriginalValue`DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="e0221-220">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="e0221-221">Aşağıdaki kod, istemci (Bu yönteme gönderilen değerler) ve DB değerlerini alır:</span><span class="sxs-lookup"><span data-stu-id="e0221-221">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="e0221-222">DB sahip her bir sütunun ne için deftere farklı değerler için aşağıdaki kod bir özel hata iletisi ekler `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="e0221-222">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="e0221-223">Aşağıda vurgulanan kod kümeleri `RowVersion` değerden yeni değere Veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="e0221-223">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="e0221-224">Kullanıcı, sonraki açışınızda **kaydetmek**, düzenleme sayfasını son görüntüsünü yakalanan bu yana, eşzamanlılık hataları.</span><span class="sxs-lookup"><span data-stu-id="e0221-224">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="e0221-225">`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski sahip `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="e0221-225">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="e0221-226">Razor sayfasındaki `ModelState` her ikisi de mevcut olduğunda bir alan modeli özellik değerlerini önceliklidir için bir değer.</span><span class="sxs-lookup"><span data-stu-id="e0221-226">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="e0221-227">Güncelleştirmeyi Düzenle sayfası</span><span class="sxs-lookup"><span data-stu-id="e0221-227">Update the Edit page</span></span>

<span data-ttu-id="e0221-228">Güncelleştirme *Pages/Departments/Edit.cshtml* aşağıdaki biçimlendirme ile:</span><span class="sxs-lookup"><span data-stu-id="e0221-228">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="e0221-229">Önceki biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="e0221-229">The preceding markup:</span></span>

* <span data-ttu-id="e0221-230">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="e0221-230">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="e0221-231">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="e0221-231">Adds a hidden row version.</span></span> <span data-ttu-id="e0221-232">`RowVersion`POST geri değere bağlar şekilde eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0221-232">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="e0221-233">Son baytını görüntüler `RowVersion` hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="e0221-233">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="e0221-234">Değiştirir `ViewData` kesin türü belirtilmiş ile `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="e0221-234">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="e0221-235">Test eşzamanlılık düzenleme sayfasını çakışıyor</span><span class="sxs-lookup"><span data-stu-id="e0221-235">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="e0221-236">İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="e0221-236">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="e0221-237">Uygulamayı çalıştırın ve Departmanlar seçin.</span><span class="sxs-lookup"><span data-stu-id="e0221-237">Run the app and select Departments.</span></span>
* <span data-ttu-id="e0221-238">Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="e0221-238">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="e0221-239">İlk sekmesini tıklatın **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="e0221-239">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="e0221-240">İki tarayıcı sekmeleri aynı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="e0221-240">The two browser tabs display the same information.</span></span>

<span data-ttu-id="e0221-241">İlk tarayıcı sekmesinde adını değiştirip tıklatın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="e0221-241">Change the name in the first browser tab and click **Save**.</span></span>

![Departman düzenleme değişiklikten sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="e0221-243">Tarayıcı değiştirilen değeri ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-243">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="e0221-244">Güncelleştirilmiş rowVersion göstergesi dikkat edin diğer sekmesinde ikinci geri gönderme üzerinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e0221-244">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="e0221-245">İkinci bir tarayıcı sekmesinde başka bir alanı değiştirin.</span><span class="sxs-lookup"><span data-stu-id="e0221-245">Change a different field in the second browser tab.</span></span>

![Departman düzenleme değişiklikten sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="e0221-247">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e0221-247">Click **Save**.</span></span> <span data-ttu-id="e0221-248">DB değerleri eşleşmiyor tüm alanlar için hata iletilerine bakın:</span><span class="sxs-lookup"><span data-stu-id="e0221-248">You see error messages for all fields that don't match the DB values:</span></span>

![Departman Düzenle sayfası hata iletisi](concurrency/_static/edit-error.png)

<span data-ttu-id="e0221-250">Bu tarayıcı penceresini ad alanını değiştirmek istediniz değil.</span><span class="sxs-lookup"><span data-stu-id="e0221-250">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="e0221-251">Kopyalama ve geçerli değeri (dilleri) ad alanına yapıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e0221-251">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="e0221-252">Out sekmesi. İstemci tarafı doğrulama hata iletisi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="e0221-252">Tab out. Client-side validation removes the error message.</span></span>

![Departman Düzenle sayfası hata iletisi](concurrency/_static/cv.png)

<span data-ttu-id="e0221-254">Tıklatın **kaydetmek** yeniden.</span><span class="sxs-lookup"><span data-stu-id="e0221-254">Click **Save** again.</span></span> <span data-ttu-id="e0221-255">İkinci bir tarayıcı sekmesinde girdiğiniz değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-255">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="e0221-256">Dizin Sayfası kaydedilen değerler bakın.</span><span class="sxs-lookup"><span data-stu-id="e0221-256">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="e0221-257">Güncelleştirme Sil sayfası</span><span class="sxs-lookup"><span data-stu-id="e0221-257">Update the Delete page</span></span>

<span data-ttu-id="e0221-258">Delete sayfa modeli aşağıdaki kod ile güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="e0221-258">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="e0221-259">Varlık getirildi sonra değiştiğinde Sil sayfasında eşzamanlılık çakışması algılar.</span><span class="sxs-lookup"><span data-stu-id="e0221-259">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="e0221-260">`Department.RowVersion`satır sürümü varlık getirildi durumdur.</span><span class="sxs-lookup"><span data-stu-id="e0221-260">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="e0221-261">EF çekirdek SQL DELETE komutu oluşturduğunda, bir WHERE yan tümcesi ile içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="e0221-261">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="e0221-262">Sıfır satır SQL DELETE komutu sonuçları etkilenen ise:</span><span class="sxs-lookup"><span data-stu-id="e0221-262">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="e0221-263">`RowVersion` SQL DELETE komutu eşleşmeyen `RowVersion` DB'de.</span><span class="sxs-lookup"><span data-stu-id="e0221-263">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="e0221-264">DbUpdateConcurrencyException özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="e0221-264">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="e0221-265">`OnGetAsync`çağrılır `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="e0221-265">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="e0221-266">Güncelleştirme Sil sayfası</span><span class="sxs-lookup"><span data-stu-id="e0221-266">Update the Delete page</span></span>

<span data-ttu-id="e0221-267">Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e0221-267">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="e0221-268">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="e0221-268">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="e0221-269">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="e0221-269">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="e0221-270">Bir hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="e0221-270">Adds an error message.</span></span>
* <span data-ttu-id="e0221-271">FullName içinde FirstMidName değiştirir **yönetici** alan.</span><span class="sxs-lookup"><span data-stu-id="e0221-271">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="e0221-272">Değişiklikleri `RowVersion` son bayta kalan görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="e0221-272">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="e0221-273">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="e0221-273">Adds a hidden row version.</span></span> <span data-ttu-id="e0221-274">`RowVersion`POST geri değere bağlar şekilde eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e0221-274">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="e0221-275">Test eşzamanlılık Delete sayfa çakışıyor</span><span class="sxs-lookup"><span data-stu-id="e0221-275">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="e0221-276">Bir test bölüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e0221-276">Create a test department.</span></span>

<span data-ttu-id="e0221-277">İki tarayıcılar örneklerinde Delete test departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="e0221-277">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="e0221-278">Uygulamayı çalıştırın ve Departmanlar seçin.</span><span class="sxs-lookup"><span data-stu-id="e0221-278">Run the app and select Departments.</span></span>
* <span data-ttu-id="e0221-279">Sağ **silmek** seçin ve test departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="e0221-279">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="e0221-280">Tıklatın **Düzenle** test departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="e0221-280">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="e0221-281">İki tarayıcı sekmeleri aynı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="e0221-281">The two browser tabs display the same information.</span></span>

<span data-ttu-id="e0221-282">İlk tarayıcı sekmesinde bütçe değiştirip'ı **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="e0221-282">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="e0221-283">Tarayıcı değiştirilen değeri ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="e0221-283">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="e0221-284">Güncelleştirilmiş rowVersion göstergesi dikkat edin diğer sekmesinde ikinci geri gönderme üzerinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e0221-284">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="e0221-285">Test departmanı ikinci sekmesinden silin. Bir eşzamanlılık hatası DB'den geçerli değerlerle görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e0221-285">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="e0221-286">Tıklatarak **silmek** sürece varlığı silen `RowVersion` updated.department silinmiş olmuştur.</span><span class="sxs-lookup"><span data-stu-id="e0221-286">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="e0221-287">Bkz: [devralma](xref:data/ef-mvc/inheritance) devralma veri modelindeki nasıl yönerge için.</span><span class="sxs-lookup"><span data-stu-id="e0221-287">See [Inheritance](xref:data/ef-mvc/inheritance) for instruction on how to inheritance in the data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="e0221-288">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e0221-288">Additional resources</span></span>

* [<span data-ttu-id="e0221-289">EF çekirdek eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="e0221-289">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="e0221-290">EF çekirdek eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="e0221-290">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="e0221-291">Önceki</span><span class="sxs-lookup"><span data-stu-id="e0221-291">Previous</span></span>](xref:data/ef-rp/update-related-data)
