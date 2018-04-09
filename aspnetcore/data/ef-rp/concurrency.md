---
title: Razor sayfalarının ASP.NET Core - eşzamanlılık - 8 8'in EF çekirdek ile
author: rick-anderson
description: Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/concurrency
ms.openlocfilehash: 1a5d1bdcb20da8270a0605c3937af2a8700a4e7f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<span data-ttu-id="3b6d8-103">en-us/</span><span class="sxs-lookup"><span data-stu-id="3b6d8-103">en-us/</span></span>

# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="3b6d8-104">Razor sayfalarının ASP.NET Core - eşzamanlılık - 8 8'in EF çekirdek ile</span><span class="sxs-lookup"><span data-stu-id="3b6d8-104">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="3b6d8-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [zel Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="3b6d8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="3b6d8-106">Bu öğretici, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="3b6d8-107">Olamaz çözmek sorunlarla karşılaşırsanız, indirme [Bu aşama için tamamlanan uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="3b6d8-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="3b6d8-108">Eşzamanlılık çakışmaları</span><span class="sxs-lookup"><span data-stu-id="3b6d8-108">Concurrency conflicts</span></span>

<span data-ttu-id="3b6d8-109">Bir eşzamanlılık çakışması ortaya çıkar zaman:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="3b6d8-110">Bir kullanıcıyı bir varlığın düzenleme sayfasına götürür.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="3b6d8-111">İlk kullanıcının değişiklik DB yazılmadan önce başka bir kullanıcı aynı varlık güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="3b6d8-112">Eşzamanlılık algılama etkin değil, eş zamanlı güncelleştirmeler olduğunda:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="3b6d8-113">Son güncelleştirme WINS.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-113">The last update wins.</span></span> <span data-ttu-id="3b6d8-114">Diğer bir deyişle, son güncelleştirme değerleri Veritabanına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="3b6d8-115">Geçerli güncelleştirmeleri ilk kaybolur.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="3b6d8-116">İyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="3b6d8-116">Optimistic concurrency</span></span>

<span data-ttu-id="3b6d8-117">İyimser eşzamanlılık eşzamanlılık çakışması olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="3b6d8-118">Örneğin, Jane departmanı Düzen sayfasını ziyaret ve bütçe İngilizce departmanı için 350,000.00 0,00 için değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

<span data-ttu-id="3b6d8-120">Jane tıklar önce **kaydetmek**, John aynı sayfasını ziyaret ve başlangıç tarihi alanı 9/1/2007'den 9/1/2013'e değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Başlangıç tarihi 2013 için değiştirme](concurrency/_static/change-date.png)

<span data-ttu-id="3b6d8-122">Jane tıklar **kaydetmek** ilk ve her tarayıcı dizin sayfası görüntülendiğinde değiştirmek görür.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

<span data-ttu-id="3b6d8-124">John tıklar **kaydetmek** bir düzenleme sayfasında $350,000.00 bütçe görüntülenmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="3b6d8-125">Ne olacağını eşzamanlılık çakışmaları nasıl işleneceğini tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="3b6d8-126">İyimser eşzamanlılık aşağıdaki seçenekleri içerir:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="3b6d8-127">Bir kullanıcı değiştirdi hangi özelliğinin izlemek ve yalnızca karşılık gelen sütunlara DB'de güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="3b6d8-128">Senaryoda, hiçbir veri kaybı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="3b6d8-129">Farklı özellikler iki kullanıcı tarafından güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="3b6d8-130">Birisi İngilizce departmanı gözatar sonraki açışınızda Jane'nın ve Can'ın değişiklikleri görürler.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="3b6d8-131">Güncelleştirme bu yöntem, veri kaybına sebep çakışmaları sayısını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="3b6d8-132">Bu yaklaşım: \* aynı özelliğe rakip bir değişiklik yaptıysanız veri kaybını önlemek olamaz.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="3b6d8-133">\* Olan bir web uygulamasında pratik genellikle.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="3b6d8-134">Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="3b6d8-135">Büyük miktarlarda durumu koruma uygulama performansını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="3b6d8-136">\* Bir varlık eşzamanlılık algılamayı karşılaştırılan uygulama karmaşıklığını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="3b6d8-137">Jane'nın değişiklik üzerine Can'ın değişiklik izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="3b6d8-138">Sonraki birisi İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="3b6d8-139">Bu yaklaşım adlı bir *istemci WINS* veya *WINS'de son* senaryo.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="3b6d8-140">(İstemci tüm değerleri veri deposunda nedir üzerinden önceliklidir.) Hiçbir eşzamanlılık işleme için kodlama yapmazsanız, istemci WINS otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="3b6d8-141">DB güncelleştirilmiş Can'ın değişiklik engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="3b6d8-142">Uygulama zamanki: \* bir hata iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="3b6d8-143">\* Verilerin geçerli durumunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="3b6d8-144">\* Değişiklikleri uygulamak verin.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-144">\* Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="3b6d8-145">Bu adlı bir *deposu WINS* senaryo.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="3b6d8-146">(Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide deposu WINS senaryo uygulayın.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="3b6d8-147">Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı üzerine yazılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="3b6d8-148">Eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="3b6d8-148">Handling concurrency</span></span> 

<span data-ttu-id="3b6d8-149">Olarak bir özellik yapılandırıldığında bir [eşzamanlılık belirteci](https://docs.microsoft.com/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="3b6d8-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="3b6d8-150">EF çekirdek getirildi sonra'nın özellik değiştirilmemiş doğrular.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="3b6d8-151">Onay oluşur zaman [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-151">The check occurs when [SaveChanges](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="3b6d8-152">Bunu getirildikten sonra özelliği değiştirildiğinde, bir [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) atılır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="3b6d8-153">Veritabanını ve veri modeli atma desteklemek için yapılandırılması gerekir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="3b6d8-154">Bir özelliğe eşzamanlılık çakışmalarını algılama</span><span class="sxs-lookup"><span data-stu-id="3b6d8-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="3b6d8-155">Eşzamanlılık çakışması algılanan özelliği düzeyindeki [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="3b6d8-156">Öznitelik, model üzerinde birden çok özellikleri uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="3b6d8-157">Daha fazla bilgi için bkz: [veri ek açıklamaları-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="3b6d8-157">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="3b6d8-158">`[ConcurrencyCheck]` Özniteliği, bu öğreticide kullanılan değil.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-158">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="3b6d8-159">Bir satırda eşzamanlılık çakışmalarını algılama</span><span class="sxs-lookup"><span data-stu-id="3b6d8-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="3b6d8-160">Eşzamanlılık çakışması algılamak için bir [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) sütun izleme modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-160">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="3b6d8-161">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="3b6d8-161">`rowversion` :</span></span>

* <span data-ttu-id="3b6d8-162">SQL Server özeldir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-162">Is SQL Server specific.</span></span> <span data-ttu-id="3b6d8-163">Diğer veritabanlarını benzer bir özellik sağlamayabilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="3b6d8-164">DB'den getirildikten sonra'nın bir varlık değiştirilmedi belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="3b6d8-165">DB bir sıralı oluşturur `rowversion` her satırın artar numarası güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="3b6d8-166">İçinde bir `Update` veya `Delete` komutu, `Where` yan tümcesi içeren getirilen değeri `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="3b6d8-167">Güncelleştirilen satır değiştiyse:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="3b6d8-168">`rowversion` getirilen değerle eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="3b6d8-169">`Update` Veya `Delete` olduğundan, komutları bir satır bulmak yok `Where` yan tümcesi içeren getirilen `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="3b6d8-170">A `DbUpdateConcurrencyException` atılır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="3b6d8-171">EF tarafından hiçbir satır güncelleştirildiğinde çekirdek içinde bir `Update` veya `Delete` komutu, bir eşzamanlılık özel durumu oluşur.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="3b6d8-172">Departman varlığa izleme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="3b6d8-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="3b6d8-173">İçinde *Models/Department.cs*, RowVersion adlı bir izleme özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="3b6d8-174">[Zaman damgası](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği belirtir. Bu sütunda yer `Where` yan tümcesi `Update` ve `Delete` komutları.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-174">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="3b6d8-175">Öznitelik adı verilen `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` önce SQL veri türü `rowversion` türü değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="3b6d8-176">Fluent API izleme özelliği de belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="3b6d8-177">Aşağıdaki kod bir bölüm adı güncelleştirildiğinde EF çekirdek tarafından oluşturulan T-SQL bölümünü gösterir:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="3b6d8-178">Vurgulanmış kodu gösterir önceki `WHERE` yan tümcesi içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="3b6d8-179">Varsa DB `RowVersion` eşit olmayan `RowVersion` parametre (`@p2`), satır güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="3b6d8-180">Aşağıdaki vurgulanmış kodu tam olarak bir satır güncelleştirildi doğrular T-SQL gösterir:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="3b6d8-181">[@@ROWCOUNT ](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satırların sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-181">[@@ROWCOUNT](https://docs.microsoft.com/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="3b6d8-182">Hayır, satırları güncelleştirilir, EF çekirdek oluşturur bir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="3b6d8-183">Visual Studio çıktı penceresinde T-SQL EF çekirdeği oluşturur görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="3b6d8-184">DB güncelleştir</span><span class="sxs-lookup"><span data-stu-id="3b6d8-184">Update the DB</span></span>

<span data-ttu-id="3b6d8-185">Ekleme `RowVersion` özelliğini bir geçiş gerektirir DB modeli değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="3b6d8-186">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-186">Build the project.</span></span> <span data-ttu-id="3b6d8-187">Bir komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="3b6d8-188">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-188">The preceding commands:</span></span>

* <span data-ttu-id="3b6d8-189">Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="3b6d8-190">Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="3b6d8-191">Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="3b6d8-192">DB güncelleştirmek için geçiş çalışır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="3b6d8-193">İskele Departmanlar modeli</span><span class="sxs-lookup"><span data-stu-id="3b6d8-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="3b6d8-194">Visual Studio'dan çıkın.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="3b6d8-195">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="3b6d8-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3b6d8-196">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-196">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

<span data-ttu-id="3b6d8-197">Yukarıdaki komut iskelesini kurar `Department` modeli.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="3b6d8-198">Projesini Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="3b6d8-199">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-199">Build the project.</span></span> <span data-ttu-id="3b6d8-200">Derleme hataları aşağıdaki gibi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="3b6d8-201">Genel olarak değiştirmek `_context.Department` için `_context.Departments` ("s" eklemek diğer bir deyişle, `Department`).</span><span class="sxs-lookup"><span data-stu-id="3b6d8-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="3b6d8-202">7 oluşumu bulundu ve güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="3b6d8-203">Güncelleştirme Departmanlar dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="3b6d8-203">Update the Departments Index page</span></span>

<span data-ttu-id="3b6d8-204">Oluşturulan iskele altyapısı bir `RowVersion` dizin sayfası, ancak bu alan için sütun döndürmemelidir görüntülenmesi.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="3b6d8-205">Bu öğreticide son baytını `RowVersion` eşzamanlılık anlamanıza yardımcı olması için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="3b6d8-206">Son bayta kalan benzersiz olması garanti değil.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="3b6d8-207">Gerçek bir uygulama görüntüle olmayacaktır `RowVersion` veya son baytını `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="3b6d8-208">Dizin Sayfası güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-208">Update the Index page:</span></span>

* <span data-ttu-id="3b6d8-209">Dizin Departmanlar ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="3b6d8-210">Biçimlendirme içeren Değiştir `RowVersion` son baytını ile `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="3b6d8-211">FirstMidName FullName ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="3b6d8-212">Aşağıdaki biçimlendirmede güncelleştirilmiş sayfası gösterir:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="3b6d8-213">Güncelleştirmeyi Düzenle sayfası modeli</span><span class="sxs-lookup"><span data-stu-id="3b6d8-213">Update the Edit page model</span></span>

<span data-ttu-id="3b6d8-214">Güncelleştirme *pages\departments\edit.cshtml.cs* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="3b6d8-215">Bir eşzamanlılık sorunu algılamak için [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) ile güncelleştirilmiş `rowVersion` alınan varlık değeri.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-215">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="3b6d8-216">EF çekirdek özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="3b6d8-217">Hiçbir satır güncelleştirme komutu tarafından etkilenen varsa (özgün hiçbir satır sahip `RowVersion` değeri), bir `DbUpdateConcurrencyException` özel durumu oluşur.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="3b6d8-218">Önceki kod `Department.RowVersion` varlık getirildi değeri olur.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="3b6d8-219">`OriginalValue` DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="3b6d8-220">Aşağıdaki kod, istemci (Bu yönteme gönderilen değerler) ve DB değerlerini alır:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="3b6d8-221">DB sahip her bir sütunun ne için deftere farklı değerler için aşağıdaki kod bir özel hata iletisi ekler `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="3b6d8-222">Aşağıda vurgulanan kod kümeleri `RowVersion` değerden yeni değere Veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="3b6d8-223">Kullanıcı, sonraki açışınızda **kaydetmek**, düzenleme sayfasını son görüntüsünü yakalanan bu yana, eşzamanlılık hataları.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="3b6d8-224">`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski sahip `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="3b6d8-225">Razor sayfasındaki `ModelState` her ikisi de mevcut olduğunda bir alan modeli özellik değerlerini önceliklidir için bir değer.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="3b6d8-226">Güncelleştirmeyi Düzenle sayfası</span><span class="sxs-lookup"><span data-stu-id="3b6d8-226">Update the Edit page</span></span>

<span data-ttu-id="3b6d8-227">Güncelleştirme *Pages/Departments/Edit.cshtml* aşağıdaki biçimlendirme ile:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="3b6d8-228">Önceki biçimlendirme:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-228">The preceding markup:</span></span>

* <span data-ttu-id="3b6d8-229">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="3b6d8-230">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-230">Adds a hidden row version.</span></span> <span data-ttu-id="3b6d8-231">`RowVersion` POST geri değere bağlar şekilde eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="3b6d8-232">Son baytını görüntüler `RowVersion` hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="3b6d8-233">Değiştirir `ViewData` kesin türü belirtilmiş ile `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="3b6d8-234">Test eşzamanlılık düzenleme sayfasını çakışıyor</span><span class="sxs-lookup"><span data-stu-id="3b6d8-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="3b6d8-235">İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="3b6d8-236">Uygulamayı çalıştırın ve Departmanlar seçin.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="3b6d8-237">Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="3b6d8-238">İlk sekmesini tıklatın **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="3b6d8-239">İki tarayıcı sekmeleri aynı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="3b6d8-240">İlk tarayıcı sekmesinde adını değiştirip tıklatın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-240">Change the name in the first browser tab and click **Save**.</span></span>

![Departman düzenleme değişiklikten sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="3b6d8-242">Tarayıcı değiştirilen değeri ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="3b6d8-243">Güncelleştirilmiş rowVersion göstergesi dikkat edin diğer sekmesinde ikinci geri gönderme üzerinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="3b6d8-244">İkinci bir tarayıcı sekmesinde başka bir alanı değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-244">Change a different field in the second browser tab.</span></span>

![Departman düzenleme değişiklikten sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="3b6d8-246">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-246">Click **Save**.</span></span> <span data-ttu-id="3b6d8-247">DB değerleri eşleşmiyor tüm alanlar için hata iletilerine bakın:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-247">You see error messages for all fields that don't match the DB values:</span></span>

![Departman Düzenle sayfası hata iletisi](concurrency/_static/edit-error.png)

<span data-ttu-id="3b6d8-249">Bu tarayıcı penceresini ad alanı değiştirmek istiyorsanız alamadık.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="3b6d8-250">Kopyalama ve geçerli değeri (dilleri) ad alanına yapıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="3b6d8-251">Out sekmesi. İstemci tarafı doğrulama hata iletisi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-251">Tab out. Client-side validation removes the error message.</span></span>

![Departman Düzenle sayfası hata iletisi](concurrency/_static/cv.png)

<span data-ttu-id="3b6d8-253">Tıklatın **kaydetmek** yeniden.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-253">Click **Save** again.</span></span> <span data-ttu-id="3b6d8-254">İkinci bir tarayıcı sekmesinde girdiğiniz değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="3b6d8-255">Dizin Sayfası kaydedilen değerler bakın.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="3b6d8-256">Güncelleştirme Sil sayfası</span><span class="sxs-lookup"><span data-stu-id="3b6d8-256">Update the Delete page</span></span>

<span data-ttu-id="3b6d8-257">Delete sayfa modeli aşağıdaki kod ile güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="3b6d8-258">Varlık getirildi sonra değiştiğinde Sil sayfasında eşzamanlılık çakışması algılar.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="3b6d8-259">`Department.RowVersion` satır sürümü varlık getirildi durumdur.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="3b6d8-260">EF çekirdek SQL DELETE komutu oluşturduğunda, bir WHERE yan tümcesi ile içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="3b6d8-261">Sıfır satır SQL DELETE komutu sonuçları etkilenen ise:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="3b6d8-262">`RowVersion` SQL DELETE komutu eşleşmeyen `RowVersion` DB'de.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="3b6d8-263">DbUpdateConcurrencyException özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="3b6d8-264">`OnGetAsync` çağrılır `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="3b6d8-265">Güncelleştirme Sil sayfası</span><span class="sxs-lookup"><span data-stu-id="3b6d8-265">Update the Delete page</span></span>

<span data-ttu-id="3b6d8-266">Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="3b6d8-267">Önceki biçimlendirme, aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="3b6d8-268">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="3b6d8-269">Bir hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-269">Adds an error message.</span></span>
* <span data-ttu-id="3b6d8-270">FullName içinde FirstMidName değiştirir **yönetici** alan.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="3b6d8-271">Değişiklikleri `RowVersion` son bayta kalan görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="3b6d8-272">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-272">Adds a hidden row version.</span></span> <span data-ttu-id="3b6d8-273">`RowVersion` POST geri değere bağlar şekilde eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="3b6d8-274">Test eşzamanlılık Delete sayfa çakışıyor</span><span class="sxs-lookup"><span data-stu-id="3b6d8-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="3b6d8-275">Bir test bölüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-275">Create a test department.</span></span>

<span data-ttu-id="3b6d8-276">İki tarayıcılar örneklerinde Delete test departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="3b6d8-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="3b6d8-277">Uygulamayı çalıştırın ve Departmanlar seçin.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="3b6d8-278">Sağ **silmek** seçin ve test departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="3b6d8-279">Tıklatın **Düzenle** test departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="3b6d8-280">İki tarayıcı sekmeleri aynı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="3b6d8-281">İlk tarayıcı sekmesinde bütçe değiştirip'ı **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="3b6d8-282">Tarayıcı değiştirilen değeri ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="3b6d8-283">Güncelleştirilmiş rowVersion göstergesi dikkat edin diğer sekmesinde ikinci geri gönderme üzerinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="3b6d8-284">Test departmanı ikinci sekmesinden silin. Bir eşzamanlılık hatası DB'den geçerli değerlerle görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="3b6d8-285">Tıklatarak **silmek** sürece varlığı silen `RowVersion` updated.department silinmiş olmuştur.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="3b6d8-286">Bkz: [devralma](xref:data/ef-mvc/inheritance) nasıl bir veri modeli devralır.</span><span class="sxs-lookup"><span data-stu-id="3b6d8-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="3b6d8-287">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3b6d8-287">Additional resources</span></span>

* [<span data-ttu-id="3b6d8-288">EF çekirdek eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="3b6d8-288">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/ef/core/modeling/concurrency)
* [<span data-ttu-id="3b6d8-289">EF çekirdek eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="3b6d8-289">Handle concurrency in EF Core</span></span>](https://docs.microsoft.com/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="3b6d8-290">Önceki</span><span class="sxs-lookup"><span data-stu-id="3b6d8-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
