---
title: ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları
author: rick-anderson
description: Bu öğreticide, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: 944e746624bf5fe7c586a521059fa4eb34b0f1e7
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259385"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="93595-103">ASP.NET core'da - eşzamanlılık - 8 8 EF çekirdekli Razor sayfaları</span><span class="sxs-lookup"><span data-stu-id="93595-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="93595-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), ve [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="93595-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="93595-105">Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="93595-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="93595-106">Eşzamanlılık çakışmaları</span><span class="sxs-lookup"><span data-stu-id="93595-106">Concurrency conflicts</span></span>

<span data-ttu-id="93595-107">Bir eşzamanlılık çakışması ortaya olduğunda:</span><span class="sxs-lookup"><span data-stu-id="93595-107">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="93595-108">Bir kullanıcı bir varlığın düzenleme sayfasına götürür.</span><span class="sxs-lookup"><span data-stu-id="93595-108">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="93595-109">İlk kullanıcının değişikliği veritabanına yazılmadan önce, başka bir kullanıcı aynı varlığı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-109">Another user updates the same entity before the first user's change is written to the database.</span></span>

<span data-ttu-id="93595-110">Eşzamanlılık algılama etkinleştirilmemişse, veritabanını güncelleştirme son olarak diğer kullanıcının değişikliklerinin üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="93595-110">If concurrency detection isn't enabled, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="93595-111">Bu risk kabul edilebilir ise, eşzamanlılık için programlama maliyeti avantaja aykırı bir ücret verebilir.</span><span class="sxs-lookup"><span data-stu-id="93595-111">If this risk is acceptable, the cost of programming for concurrency might outweigh the benefit.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="93595-112">Kötümser eşzamanlılık (kilitleme)</span><span class="sxs-lookup"><span data-stu-id="93595-112">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="93595-113">Eşzamanlılık çakışmalarını önlemenin bir yolu veritabanı kilitlerini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="93595-113">One way to prevent concurrency conflicts is to use database locks.</span></span> <span data-ttu-id="93595-114">Bu, Kötümser eşzamanlılık olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="93595-114">This is called pessimistic concurrency.</span></span> <span data-ttu-id="93595-115">Uygulama, güncelleştirmeyi amaçladığı bir veritabanı satırını okumadan önce bir kilit ister.</span><span class="sxs-lookup"><span data-stu-id="93595-115">Before the app reads a database row that it intends to update, it requests a lock.</span></span> <span data-ttu-id="93595-116">Bir satır, güncelleştirme erişimi için kilitlendiğinde, ilk kilit yayımlanıncaya kadar başka hiçbir kullanıcının satırı kilitlemesine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="93595-116">Once a row is locked for update access, no other users are allowed to lock the row until the first lock is released.</span></span>

<span data-ttu-id="93595-117">Kilitleri yönetmek dezavantajlara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="93595-117">Managing locks has disadvantages.</span></span> <span data-ttu-id="93595-118">Program, karmaşık olabilir ve Kullanıcı sayısı arttıkça performans sorunlarına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="93595-118">It can be complex to program and can cause performance problems as the number of users increases.</span></span> <span data-ttu-id="93595-119">Entity Framework Core, BT için yerleşik destek sağlamaz ve bu öğretici Bu öğreticinin nasıl uygulanacağını göstermez.</span><span class="sxs-lookup"><span data-stu-id="93595-119">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="93595-120">İyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="93595-120">Optimistic concurrency</span></span>

<span data-ttu-id="93595-121">İyimser eşzamanlılık eşzamanlılık çakışmalarını olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın.</span><span class="sxs-lookup"><span data-stu-id="93595-121">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="93595-122">Örneğin, Jane departmanı Düzen sayfasını ziyaret ve İngilizce departmanı için bütçe 350,000.00 $0,00 değiştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-122">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget30.png)

<span data-ttu-id="93595-124">Jane tıkladığında önce **Kaydet**, John aynı sayfayı ziyaret eder ve alanın başlangıç tarihi 1/9/2013 1/9/2007'deki değiştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-124">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date30.png)

<span data-ttu-id="93595-126">Gamze önce **Kaydet** ' i tıklatır ve değişiklik etkin duruma geçer çünkü tarayıcı Dizin sayfasını bütçe miktarı olarak sıfır ile görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="93595-126">Jane clicks **Save** first and sees her change take effect, since the browser displays the Index page with zero as the Budget amount.</span></span>

<span data-ttu-id="93595-127">John tıkladığında **Kaydet** yine de bir bütçe $350,000.00 birini gösteren bir Düzenle sayfasında.</span><span class="sxs-lookup"><span data-stu-id="93595-127">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="93595-128">Sonraki durum eşzamanlılık çakışmalarını nasıl işleydiğinize göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="93595-128">What happens next is determined by how you handle concurrency conflicts:</span></span>

* <span data-ttu-id="93595-129">Bir kullanıcının hangi özelliği değiştirdiği ve yalnızca ilgili sütunları veritabanında güncelleştirdiğinden haberdar olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-129">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

  <span data-ttu-id="93595-130">Bu senaryoda, veri kaybolacak.</span><span class="sxs-lookup"><span data-stu-id="93595-130">In the scenario, no data would be lost.</span></span> <span data-ttu-id="93595-131">Farklı özellikler iki kullanıcı tarafından güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="93595-131">Different properties were updated by the two users.</span></span> <span data-ttu-id="93595-132">Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler.</span><span class="sxs-lookup"><span data-stu-id="93595-132">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="93595-133">Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-133">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="93595-134">Bu yaklaşım bazı dezavantajlara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="93595-134">This approach has some disadvantages:</span></span>
 
  * <span data-ttu-id="93595-135">Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.</span><span class="sxs-lookup"><span data-stu-id="93595-135">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="93595-136">Genellikle bir web uygulaması pratik değildir.</span><span class="sxs-lookup"><span data-stu-id="93595-136">Is generally not practical in a web app.</span></span> <span data-ttu-id="93595-137">Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="93595-137">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="93595-138">Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="93595-138">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="93595-139">Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-139">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="93595-140">Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-140">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="93595-141">Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri.</span><span class="sxs-lookup"><span data-stu-id="93595-141">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="93595-142">Bu yaklaşım olarak adlandırılan bir *istemci WINS* veya *WINS'te son* senaryo.</span><span class="sxs-lookup"><span data-stu-id="93595-142">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="93595-143">(İstemciden gelen tüm değerler veri deposunda yer alacak şekilde önceliklidir.) Eşzamanlılık işleme için herhangi bir kodlama yapmazsanız, Istemci WINS otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="93595-143">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="93595-144">John 'un değişikliğini veritabanında güncelleştirilmesini engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-144">You can prevent John's change from being updated in the database.</span></span> <span data-ttu-id="93595-145">Genellikle, bir uygulamayı atadığınız:</span><span class="sxs-lookup"><span data-stu-id="93595-145">Typically, the app would:</span></span>

  * <span data-ttu-id="93595-146">Bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="93595-146">Display an error message.</span></span>
  * <span data-ttu-id="93595-147">Verilerin geçerli durumunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="93595-147">Show the current state of the data.</span></span>
  * <span data-ttu-id="93595-148">Değişiklikleri uygulamak verin.</span><span class="sxs-lookup"><span data-stu-id="93595-148">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="93595-149">Bu adlı bir *Store WINS* senaryo.</span><span class="sxs-lookup"><span data-stu-id="93595-149">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="93595-150">(Veri deposu değerleri, istemci tarafından gönderilen değerlere göre önceliklidir.) Bu öğreticide mağaza WINS senaryosunu uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="93595-150">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="93595-151">Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="93595-151">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="conflict-detection-in-ef-core"></a><span data-ttu-id="93595-152">EF Core çakışma algılama</span><span class="sxs-lookup"><span data-stu-id="93595-152">Conflict detection in EF Core</span></span>

<span data-ttu-id="93595-153">EF Core, çakışmalar algıladığında `DbConcurrencyException` özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93595-153">EF Core throws `DbConcurrencyException` exceptions when it detects conflicts.</span></span> <span data-ttu-id="93595-154">Çakışma algılamayı etkinleştirmek için veri modeli yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="93595-154">The data model has to be configured to enable conflict detection.</span></span> <span data-ttu-id="93595-155">Çakışma algılamayı etkinleştirme seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="93595-155">Options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="93595-156">EF Core, Update ve DELETE komutlarının WHERE yan tümcesinde [eşzamanlılık belirteçleri](/ef/core/modeling/concurrency) olarak yapılandırılan sütunların özgün değerlerini içerecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="93595-156">Configure EF Core to include the original values of columns configured as [concurrency tokens](/ef/core/modeling/concurrency) in the Where clause of Update and Delete commands.</span></span>

  <span data-ttu-id="93595-157">`SaveChanges` çağrıldığında WHERE yan tümcesi [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) özniteliğiyle açıklama eklenmiş herhangi bir özelliğin özgün değerlerini arar.</span><span class="sxs-lookup"><span data-stu-id="93595-157">When `SaveChanges` is called, the Where clause looks for the original values of any properties annotated with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute) attribute.</span></span> <span data-ttu-id="93595-158">Update deyimleri, satır ilk okunduğundan bu yana eşzamanlılık belirteci özelliklerinden herhangi biri değiştiyse güncelleştirilecek bir satır bulmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="93595-158">The update statement won't find a row to update if any of the concurrency token properties changed since the row was first read.</span></span> <span data-ttu-id="93595-159">EF Core eşzamanlılık çakışması olarak yorumlar.</span><span class="sxs-lookup"><span data-stu-id="93595-159">EF Core interprets that as a concurrency conflict.</span></span> <span data-ttu-id="93595-160">Birçok sütunu olan veritabanı tablolarında bu yaklaşım çok büyük WHERE yan tümceleriyle sonuçlanabilir ve büyük miktarlarda durum gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="93595-160">For database tables that have many columns, this approach can result in very large Where clauses, and can require large amounts of state.</span></span> <span data-ttu-id="93595-161">Bu nedenle bu yaklaşım genellikle önerilmez ve bu öğreticide kullanılan yöntem değildir.</span><span class="sxs-lookup"><span data-stu-id="93595-161">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

* <span data-ttu-id="93595-162">Veritabanı tablosunda, bir satırın ne zaman değiştirildiğini belirlemede kullanılabilecek bir izleme sütunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="93595-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span>

  <span data-ttu-id="93595-163">SQL Server veritabanında, izleme sütununun veri türü `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="93595-163">In a SQL Server database, the data type of the tracking column is `rowversion`.</span></span> <span data-ttu-id="93595-164">`rowversion` değeri, satır her güncelleştirildiği zaman artılan sıralı bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="93595-164">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="93595-165">Bir Update veya delete komutunda WHERE yan tümcesi, izleme sütununun (orijinal satır sürüm numarası) orijinal değerini içerir.</span><span class="sxs-lookup"><span data-stu-id="93595-165">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version number).</span></span> <span data-ttu-id="93595-166">Güncelleştirilmekte olan satır başka bir kullanıcı tarafından değiştirilmişse, `rowversion` sütunundaki değer özgün değerden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="93595-166">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value.</span></span> <span data-ttu-id="93595-167">Bu durumda, Update veya DELETE deyimi WHERE yan tümcesi nedeniyle güncelleştirilecek satırı bulamıyor.</span><span class="sxs-lookup"><span data-stu-id="93595-167">In that case, the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="93595-168">Bir Update veya delete komutundan hiçbir satır etkilenmeden EF Core eşzamanlılık özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93595-168">EF Core throws a concurrency exception when no rows are affected by an Update or Delete command.</span></span>

## <a name="add-a-tracking-property"></a><span data-ttu-id="93595-169">İzleme özelliği Ekle</span><span class="sxs-lookup"><span data-stu-id="93595-169">Add a tracking property</span></span>

<span data-ttu-id="93595-170">İçinde *Models/Department.cs*, RowVersion adlı izleme özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="93595-170">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

<span data-ttu-id="93595-171">[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği sütunu eşzamanlılık izleme sütunu olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="93595-171">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute is what identifies the column as a concurrency tracking column.</span></span> <span data-ttu-id="93595-172">Fluent API, izleme özelliğini belirtmenin alternatif bir yoludur:</span><span class="sxs-lookup"><span data-stu-id="93595-172">The fluent API is an alternative way to specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93595-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93595-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="93595-174">Bir SQL Server veritabanı için, bayt dizisi olarak tanımlanan bir varlık özelliğindeki `[Timestamp]` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="93595-174">For a SQL Server database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="93595-175">Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="93595-175">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="93595-176">Veritabanındaki sütun türünü [rowversion](/sql/t-sql/data-types/rowversion-transact-sql)olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="93595-176">Sets the column type in the database to [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span></span>

<span data-ttu-id="93595-177">Veritabanı, satır her güncelleştirildiği zaman artılan sıralı bir satır sürüm numarası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93595-177">The database generates a sequential row version number that's incremented each time the row is updated.</span></span> <span data-ttu-id="93595-178">Bir `Update` veya `Delete` komutunda, `Where` yan tümcesi getirilen satır sürümü değerini içerir.</span><span class="sxs-lookup"><span data-stu-id="93595-178">In an `Update` or `Delete` command, the `Where` clause includes the fetched row version value.</span></span> <span data-ttu-id="93595-179">Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:</span><span class="sxs-lookup"><span data-stu-id="93595-179">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="93595-180">Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="93595-180">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="93595-181">`Where` yan tümcesi getirilen satır sürümü değerini aradığı için `Update` veya `Delete` komutları bir satır bulamaz.</span><span class="sxs-lookup"><span data-stu-id="93595-181">The `Update` or `Delete` commands don't find a row because the `Where` clause looks for the fetched row version value.</span></span>
* <span data-ttu-id="93595-182">A `DbUpdateConcurrencyException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-182">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="93595-183">Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="93595-183">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

<span data-ttu-id="93595-184">Önceki kodun gösterdiği vurgulanmış `WHERE` yan tümcesi içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="93595-184">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="93595-185">Veritabanı `RowVersion` `RowVersion` parametresine eşit değilse (`@p2`), hiçbir satır güncellenmez.</span><span class="sxs-lookup"><span data-stu-id="93595-185">If the database `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="93595-186">Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:</span><span class="sxs-lookup"><span data-stu-id="93595-186">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

<span data-ttu-id="93595-187">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satır sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="93595-187">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="93595-188">Hiçbir satır güncellenmemişse, EF Core bir `DbUpdateConcurrencyException`oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93595-188">If no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93595-189">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93595-189">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="93595-190">Bir SQLite veritabanı için, bir varlık özelliğindeki `[Timestamp]` özniteliği byte dizisi olarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="93595-190">For a SQLite database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="93595-191">Sütunun WHERE yan tümceleri DELETE ve UPDATE 'e dahil edilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="93595-191">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="93595-192">BLOB sütun türüyle eşlenir.</span><span class="sxs-lookup"><span data-stu-id="93595-192">Maps to a BLOB column type.</span></span>

<span data-ttu-id="93595-193">Veritabanı Tetikleyicileri satır her güncelleştirildiğinde, RowVersion sütununu yeni bir rastgele bayt dizisiyle güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-193">Database triggers update the RowVersion column with a new random byte array whenever a row is updated.</span></span> <span data-ttu-id="93595-194">Bir `Update` veya `Delete` komutunda, `Where` yan tümcesi RowVersion sütununun getirilen değerini içerir.</span><span class="sxs-lookup"><span data-stu-id="93595-194">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of the RowVersion column.</span></span> <span data-ttu-id="93595-195">Güncelleştirilmekte olan satır getirilmesinden sonra değiştiyse:</span><span class="sxs-lookup"><span data-stu-id="93595-195">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="93595-196">Geçerli satır sürümü değeri getirilen değerle eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="93595-196">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="93595-197">`Where` yan tümcesi orijinal satır sürümü değerini aradığı için `Update` veya `Delete` komutu bir satır bulamaz.</span><span class="sxs-lookup"><span data-stu-id="93595-197">The `Update` or `Delete` command doesn't find a row because the `Where` clause looks for the original row version value.</span></span>
* <span data-ttu-id="93595-198">A `DbUpdateConcurrencyException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-198">A `DbUpdateConcurrencyException` is thrown.</span></span>

---

### <a name="update-the-database"></a><span data-ttu-id="93595-199">Veritabanını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="93595-199">Update the database</span></span>

<span data-ttu-id="93595-200">`RowVersion` özelliği eklendiğinde geçiş gerektiren veri modeli değişir.</span><span class="sxs-lookup"><span data-stu-id="93595-200">Adding the `RowVersion` property changes the data model, which requires a migration.</span></span>

<span data-ttu-id="93595-201">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-201">Build the project.</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93595-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93595-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="93595-203">PMC 'de şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="93595-203">Run the following command in the PMC:</span></span>

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93595-204">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93595-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="93595-205">Bir terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="93595-205">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

<span data-ttu-id="93595-206">Bu komut:</span><span class="sxs-lookup"><span data-stu-id="93595-206">This command:</span></span>

* <span data-ttu-id="93595-207">*Geçişleri/{zaman damgası} _RowVersion. cs* geçiş dosyasını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="93595-207">Creates the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="93595-208">Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="93595-208">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="93595-209">Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="93595-209">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93595-210">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93595-210">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="93595-211">PMC 'de şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="93595-211">Run the following command in the PMC:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93595-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93595-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="93595-213">`Migrations/<timestamp>_RowVersion.cs` dosyasını açın ve vurgulanan kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="93595-213">Open the `Migrations/<timestamp>_RowVersion.cs` file and add the highlighted code:</span></span>

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  <span data-ttu-id="93595-214">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="93595-214">The preceding code:</span></span>

  * <span data-ttu-id="93595-215">Mevcut satırları rastgele blob değerleriyle güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-215">Updates existing rows with random blob values.</span></span>
  * <span data-ttu-id="93595-216">Satır her güncelleştirildiğinde ROWVERSION sütununu rastgele bir blob değerine ayarlamış veritabanı Tetikleyicileri ekler.</span><span class="sxs-lookup"><span data-stu-id="93595-216">Adds database triggers that set the RowVersion column to a random blob value whenever a row is updated.</span></span>

* <span data-ttu-id="93595-217">Bir terminalde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="93595-217">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a><span data-ttu-id="93595-218">Yapı iskelesi departmanı sayfaları</span><span class="sxs-lookup"><span data-stu-id="93595-218">Scaffold Department pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93595-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93595-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="93595-220">Aşağıdaki özel durumlarla birlikte [Yapı Fkatlama öğrenci sayfalarındaki](xref:data/ef-rp/intro#scaffold-student-pages) yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="93595-220">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

* <span data-ttu-id="93595-221">*Sayfalar/departmanlar* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-221">Create a *Pages/Departments* folder.</span></span>  
* <span data-ttu-id="93595-222">Model sınıfı için `Department` kullanın.</span><span class="sxs-lookup"><span data-stu-id="93595-222">Use `Department` for the model class.</span></span>
  * <span data-ttu-id="93595-223">Yeni bir tane oluşturmak yerine var olan bağlam sınıfını kullanın.</span><span class="sxs-lookup"><span data-stu-id="93595-223">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93595-224">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93595-224">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="93595-225">*Sayfalar/departmanlar* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-225">Create a *Pages/Departments* folder.</span></span>

* <span data-ttu-id="93595-226">Bölüm sayfalarını iskele almak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="93595-226">Run the following command to scaffold the Department pages.</span></span>

  <span data-ttu-id="93595-227">**Windows üzerinde:**</span><span class="sxs-lookup"><span data-stu-id="93595-227">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

  <span data-ttu-id="93595-228">**Linux veya macOS 'ta:**</span><span class="sxs-lookup"><span data-stu-id="93595-228">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages/Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="93595-229">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-229">Build the project.</span></span>

## <a name="update-the-index-page"></a><span data-ttu-id="93595-230">Dizin sayfasını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="93595-230">Update the Index page</span></span>

<span data-ttu-id="93595-231">Scafkatlama aracı Dizin sayfası için bir `RowVersion` sütunu oluşturdu, ancak bu alan bir üretim uygulamasında gösterilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="93595-231">The scaffolding tool created a `RowVersion` column for the Index page, but that field wouldn't be displayed in a production app.</span></span> <span data-ttu-id="93595-232">Bu öğreticide, eşzamanlılık işlemenin nasıl çalıştığını göstermeye yardımcı olmak için `RowVersion` son baytı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-232">In this tutorial, the last byte of the `RowVersion` is displayed to help show how concurrency handling works.</span></span> <span data-ttu-id="93595-233">Son baytın kendi kendine benzersiz olması garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="93595-233">The last byte isn't guaranteed to be unique by itself.</span></span>

<span data-ttu-id="93595-234">*Pages\Departments\Index.cshtml* sayfasını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="93595-234">Update *Pages\Departments\Index.cshtml* page:</span></span>

* <span data-ttu-id="93595-235">Dizin bölümlerine değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-235">Replace Index with Departments.</span></span>
* <span data-ttu-id="93595-236">Yalnızca bayt dizisinin son baytını göstermek için `RowVersion` içeren kodu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-236">Change the code containing `RowVersion` to show just the last byte of the byte array.</span></span>
* <span data-ttu-id="93595-237">FirstMidName tam adı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-237">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="93595-238">Aşağıdaki kod, güncelleştirilmiş sayfayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="93595-238">The following code shows the updated page:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a><span data-ttu-id="93595-239">Düzenleme sayfa modeli güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="93595-239">Update the Edit page model</span></span>

<span data-ttu-id="93595-240">Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="93595-240">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

<span data-ttu-id="93595-241">[OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) , `OnGet` yönteminde alındığı sırada varlıktaki `rowVersion` değeriyle güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="93595-241">The [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity when it was fetched in the `OnGet` method.</span></span> <span data-ttu-id="93595-242">EF Core özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="93595-242">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="93595-243">Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), `DbUpdateConcurrencyException` özel durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-243">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

<span data-ttu-id="93595-244">Vurgulanan kodda:</span><span class="sxs-lookup"><span data-stu-id="93595-244">In the preceding highlighted code:</span></span>

* <span data-ttu-id="93595-245">`Department.RowVersion` değeri, ilk olarak düzenleme sayfasına yönelik GET isteğine getirilen varlıkta yer alır.</span><span class="sxs-lookup"><span data-stu-id="93595-245">The value in `Department.RowVersion` is what was in the entity when it was originally fetched in the Get request for the Edit page.</span></span> <span data-ttu-id="93595-246">Değer, düzenlenecek varlığı görüntüleyen Razor sayfasındaki gizli bir alan tarafından `OnPost` yöntemine sağlanır.</span><span class="sxs-lookup"><span data-stu-id="93595-246">The value is provided to the `OnPost` method by a hidden field in the Razor page that displays the entity to be edited.</span></span> <span data-ttu-id="93595-247">Gizli alan değeri model cildi tarafından `Department.RowVersion` ' a kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="93595-247">The hidden field value is copied to `Department.RowVersion` by the model binder.</span></span>
* <span data-ttu-id="93595-248">`OriginalValue` WHERE yan tümcesinde kullanılacak EF Core.</span><span class="sxs-lookup"><span data-stu-id="93595-248">`OriginalValue` is what EF Core will use in the Where clause.</span></span> <span data-ttu-id="93595-249">Vurgulanan kod satırı çalıştırılmadan önce, `FirstOrDefaultAsync` Bu yöntemde çağrıldığında, düzenleme sayfasında görüntülendiklerden farklı olabilen `OriginalValue` veritabanında bulunan değeri vardır.</span><span class="sxs-lookup"><span data-stu-id="93595-249">Before the highlighted line of code executes, `OriginalValue` has the value that was in the database when `FirstOrDefaultAsync` was called in this method, which might be different from what was displayed on the Edit page.</span></span>
* <span data-ttu-id="93595-250">Vurgulanan kod, EF Core SQL UPDATE ifadesinin WHERE yan tümcesindeki görüntülenen `Department` varlığındaki özgün `RowVersion` değerini kullandığından emin olur.</span><span class="sxs-lookup"><span data-stu-id="93595-250">The highlighted code makes sure that EF Core uses the original `RowVersion` value from the displayed `Department` entity in the SQL UPDATE statement's Where clause.</span></span>

<span data-ttu-id="93595-251">Bir eşzamanlılık hatası oluştuğunda, aşağıdaki vurgulanmış kod istemci değerlerini (Bu yönteme gönderilen değerler) ve veritabanı değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="93595-251">When a concurrency error happens, the following highlighted code gets the client values (the values posted to this method) and the database values.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

<span data-ttu-id="93595-252">Aşağıdaki kod, `OnPostAsync`gönderilen değerden farklı veritabanı değerleri olan her bir sütun için özel bir hata iletisi ekler:</span><span class="sxs-lookup"><span data-stu-id="93595-252">The following code adds a custom error message for each column that has database values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

<span data-ttu-id="93595-253">Aşağıdaki vurgulanan kod, `RowVersion` değerini veritabanından alınan yeni değer olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="93595-253">The following highlighted code sets the `RowVersion` value to the new value retrieved from the database.</span></span> <span data-ttu-id="93595-254">Kullanıcının bir sonraki tıklayışında **Kaydet**, düzenleme sayfası son görüntülenmesini yakalandı beri gerçekleşen eşzamanlılık hataları.</span><span class="sxs-lookup"><span data-stu-id="93595-254">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

<span data-ttu-id="93595-255">`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski olan `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="93595-255">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="93595-256">Razor Sayfası'nda `ModelState` değerinin ikisi de mevcut olduğunda alan üzerinde model özellik değerlerini öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="93595-256">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

### <a name="update-the-razor-page"></a><span data-ttu-id="93595-257">Razor sayfasını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="93595-257">Update the Razor page</span></span>

<span data-ttu-id="93595-258">*Sayfaları/departmanları/Düzenle. cshtml* 'yi aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="93595-258">Update *Pages/Departments/Edit.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="93595-259">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="93595-259">The preceding code:</span></span>

* <span data-ttu-id="93595-260">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="93595-260">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="93595-261">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="93595-261">Adds a hidden row version.</span></span> <span data-ttu-id="93595-262">`RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="93595-262">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="93595-263">Son baytı görüntüler `RowVersion` hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="93595-263">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="93595-264">Değiştirir `ViewData` türü kesin belirlenmiş ile `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="93595-264">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

### <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="93595-265">Eşzamanlılık çakışmalarını düzenleme sayfası ile test</span><span class="sxs-lookup"><span data-stu-id="93595-265">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="93595-266">İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="93595-266">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="93595-267">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="93595-267">Run the app and select Departments.</span></span>
* <span data-ttu-id="93595-268">Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="93595-268">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="93595-269">Birinci sekmede tıklayın **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="93595-269">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="93595-270">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="93595-270">The two browser tabs display the same information.</span></span>

<span data-ttu-id="93595-271">' A tıklayın ve ilk tarayıcı sekmesine adını değiştirmek **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="93595-271">Change the name in the first browser tab and click **Save**.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-130.png)

<span data-ttu-id="93595-273">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="93595-273">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="93595-274">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-274">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="93595-275">İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-275">Change a different field in the second browser tab.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-230.png)

<span data-ttu-id="93595-277">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="93595-277">Click **Save**.</span></span> <span data-ttu-id="93595-278">Veritabanı değerleriyle eşleşmeyen tüm alanlar için hata iletileri görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="93595-278">You see error messages for all fields that don't match the database values:</span></span>

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error30.png)

<span data-ttu-id="93595-280">Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-280">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="93595-281">Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="93595-281">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="93595-282">Sekme genişletme. İstemci tarafı doğrulaması hata iletisini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="93595-282">Tab out. Client-side validation removes the error message.</span></span>

<span data-ttu-id="93595-283">Tıklayın **Kaydet** yeniden.</span><span class="sxs-lookup"><span data-stu-id="93595-283">Click **Save** again.</span></span> <span data-ttu-id="93595-284">İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="93595-284">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="93595-285">Dizin sayfasında kaydedilen değerler görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="93595-285">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="93595-286">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="93595-286">Update the Delete page</span></span>

<span data-ttu-id="93595-287">*Sayfaları/departmanları/delete. cshtml. cs* öğesini aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="93595-287">Update *Pages/Departments/Delete.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="93595-288">Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar.</span><span class="sxs-lookup"><span data-stu-id="93595-288">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="93595-289">`Department.RowVersion` Varlık getirildi satır sürümü andır.</span><span class="sxs-lookup"><span data-stu-id="93595-289">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="93595-290">EF Core SQL DELETE komutu oluşturduğunda, WHERE yan tümcesi ile içerdiği `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="93595-290">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="93595-291">Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:</span><span class="sxs-lookup"><span data-stu-id="93595-291">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="93595-292">SQL DELETE komutundaki `RowVersion` veritabanındaki `RowVersion` eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="93595-292">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the database.</span></span>
* <span data-ttu-id="93595-293">DbUpdateConcurrencyException özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-293">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="93595-294">`OnGetAsync` çağrılır `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="93595-294">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="93595-295">Razor Sil sayfasını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="93595-295">Update the Delete Razor page</span></span>

<span data-ttu-id="93595-296">Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="93595-296">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="93595-297">Yukarıdaki kod aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="93595-297">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="93595-298">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="93595-298">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="93595-299">Bir hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="93595-299">Adds an error message.</span></span>
* <span data-ttu-id="93595-300">FullName FirstMidName değiştirir **yönetici** alan.</span><span class="sxs-lookup"><span data-stu-id="93595-300">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="93595-301">Değişiklikleri `RowVersion` son bayt görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="93595-301">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="93595-302">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="93595-302">Adds a hidden row version.</span></span> <span data-ttu-id="93595-303">`RowVersion` eklenmesi gerekir, çünkü postgit ekleme geri eklemesi değeri bağlar.</span><span class="sxs-lookup"><span data-stu-id="93595-303">`RowVersion` must be added so postgit add back binds the value.</span></span>

### <a name="test-concurrency-conflicts"></a><span data-ttu-id="93595-304">Eşzamanlılık çakışmalarını test et</span><span class="sxs-lookup"><span data-stu-id="93595-304">Test concurrency conflicts</span></span>

<span data-ttu-id="93595-305">Test bölümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-305">Create a test department.</span></span>

<span data-ttu-id="93595-306">İki tarayıcılar örneklerinde DELETE test departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="93595-306">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="93595-307">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="93595-307">Run the app and select Departments.</span></span>
* <span data-ttu-id="93595-308">Sağ **Sil** seçin ve test departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="93595-308">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="93595-309">Tıklayın **Düzenle** köprü test bölümü için.</span><span class="sxs-lookup"><span data-stu-id="93595-309">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="93595-310">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="93595-310">The two browser tabs display the same information.</span></span>

<span data-ttu-id="93595-311">İlk tarayıcı sekmesine bütçede değiştirip'ı **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="93595-311">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="93595-312">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="93595-312">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="93595-313">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-313">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="93595-314">İkinci sekmeden test departmanını silin. Veritabanında geçerli değerlerle birlikte bir eşzamanlılık hatası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-314">Delete the test department from the second tab. A concurrency error is display with the current values from the database.</span></span> <span data-ttu-id="93595-315">Tıklayarak **Sil** sürece varlığını siler `RowVersion` updated.department silinmiş olmuştur.</span><span class="sxs-lookup"><span data-stu-id="93595-315">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93595-316">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="93595-316">Additional resources</span></span>

* [<span data-ttu-id="93595-317">EF Core eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="93595-317">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="93595-318">EF Core eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="93595-318">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="93595-319">2. x kaynağı ASP.NET Core hata ayıklaması</span><span class="sxs-lookup"><span data-stu-id="93595-319">Debugging ASP.NET Core 2.x source</span></span>](https://github.com/aspnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a><span data-ttu-id="93595-320">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="93595-320">Next steps</span></span>

<span data-ttu-id="93595-321">Bu, serideki son öğreticidir.</span><span class="sxs-lookup"><span data-stu-id="93595-321">This is the last tutorial in the series.</span></span> <span data-ttu-id="93595-322">[Bu öğretici serisinin MVC sürümünde](xref:data/ef-mvc/index)ek konular ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="93595-322">Additional topics are covered in the [MVC version of this tutorial series](xref:data/ef-mvc/index).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="93595-323">Önceki öğretici</span><span class="sxs-lookup"><span data-stu-id="93595-323">Previous tutorial</span></span>](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="93595-324">Bu öğreticide, birden çok kullanıcı aynı anda (aynı anda) bir varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="93595-324">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="93595-325">Olamaz çözmenize, sorunlarla karşılaşırsanız, [indirin veya tamamlanmış uygulamayı görüntüleyin.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="93595-325">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="93595-326">[Yükleme yönergeleri](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="93595-326">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="93595-327">Eşzamanlılık çakışmaları</span><span class="sxs-lookup"><span data-stu-id="93595-327">Concurrency conflicts</span></span>

<span data-ttu-id="93595-328">Bir eşzamanlılık çakışması ortaya olduğunda:</span><span class="sxs-lookup"><span data-stu-id="93595-328">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="93595-329">Bir kullanıcı bir varlığın düzenleme sayfasına götürür.</span><span class="sxs-lookup"><span data-stu-id="93595-329">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="93595-330">İlk kullanıcının değişiklik DB'ye yazılmadan önce başka bir kullanıcı aynı varlık güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-330">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="93595-331">Eşzamanlılık algılama etkin değil, eş zamanlı güncelleştirmeler olduğunda:</span><span class="sxs-lookup"><span data-stu-id="93595-331">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="93595-332">Son güncelleştirme WINS.</span><span class="sxs-lookup"><span data-stu-id="93595-332">The last update wins.</span></span> <span data-ttu-id="93595-333">Diğer bir deyişle, son güncelleştirme değerleri Veritabanına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="93595-333">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="93595-334">Geçerli güncelleştirme ilk kaybolur.</span><span class="sxs-lookup"><span data-stu-id="93595-334">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="93595-335">İyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="93595-335">Optimistic concurrency</span></span>

<span data-ttu-id="93595-336">İyimser eşzamanlılık eşzamanlılık çakışmalarını olmasını sağlar ve tepki verdiğini uygun olduğunda bunlar yapın.</span><span class="sxs-lookup"><span data-stu-id="93595-336">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="93595-337">Örneğin, Jane departmanı Düzen sayfasını ziyaret ve İngilizce departmanı için bütçe 350,000.00 $0,00 değiştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-337">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

<span data-ttu-id="93595-339">Jane tıkladığında önce **Kaydet**, John aynı sayfayı ziyaret eder ve alanın başlangıç tarihi 1/9/2013 1/9/2007'deki değiştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-339">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![2013'e başlangıç tarihini değiştirme](concurrency/_static/change-date.png)

<span data-ttu-id="93595-341">Jane tıkladığında **Kaydet** ilk ve her tarayıcı dizin sayfası görüntülendiğinde değiştirme görür.</span><span class="sxs-lookup"><span data-stu-id="93595-341">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

<span data-ttu-id="93595-343">John tıkladığında **Kaydet** yine de bir bütçe $350,000.00 birini gösteren bir Düzenle sayfasında.</span><span class="sxs-lookup"><span data-stu-id="93595-343">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="93595-344">Sonraki işlemin ne eşzamanlılık çakışmalarını nasıl ele tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="93595-344">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="93595-345">İyimser eşzamanlılık aşağıdaki seçenekleri içerir:</span><span class="sxs-lookup"><span data-stu-id="93595-345">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="93595-346">Bir kullanıcı değiştirmiş hangi özelliğinin kaydını ve yalnızca ilgili sütunları DB'de güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-346">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="93595-347">Bu senaryoda, veri kaybolacak.</span><span class="sxs-lookup"><span data-stu-id="93595-347">In the scenario, no data would be lost.</span></span> <span data-ttu-id="93595-348">Farklı özellikler iki kullanıcı tarafından güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="93595-348">Different properties were updated by the two users.</span></span> <span data-ttu-id="93595-349">Biri İngilizce, departman gözatar sonraki açışınızda Gamze'nin hem Can'ın değişiklikleri görürler.</span><span class="sxs-lookup"><span data-stu-id="93595-349">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="93595-350">Bu güncelleştirme yöntemini, veri kaybına neden olabilecek çakışmaları sayısını azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-350">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="93595-351">Bu yaklaşım:</span><span class="sxs-lookup"><span data-stu-id="93595-351">This approach:</span></span>
 
  * <span data-ttu-id="93595-352">Aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.</span><span class="sxs-lookup"><span data-stu-id="93595-352">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="93595-353">Genellikle bir web uygulaması pratik değildir.</span><span class="sxs-lookup"><span data-stu-id="93595-353">Is generally not practical in a web app.</span></span> <span data-ttu-id="93595-354">Tüm getirilen ve yeni değerleri izlemek için önemli durum koruma gerektiriyor.</span><span class="sxs-lookup"><span data-stu-id="93595-354">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="93595-355">Büyük miktarlarda durumu bakımını yapma, uygulama performansını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="93595-355">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="93595-356">Bir varlıkta eşzamanlılık algılama ile karşılaştırıldığında app karmaşıklığı artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-356">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="93595-357">Gamze'nin değişikliğinin üzerine Can'ın değişiklik sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-357">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="93595-358">Sonraki biri İngilizce departmanı gözatar, 1/9/2013 görürler ve getirilen $350,000.00 değeri.</span><span class="sxs-lookup"><span data-stu-id="93595-358">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="93595-359">Bu yaklaşım olarak adlandırılan bir *istemci WINS* veya *WINS'te son* senaryo.</span><span class="sxs-lookup"><span data-stu-id="93595-359">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="93595-360">(İstemciden gelen tüm değerler veri deposunda yer alacak şekilde önceliklidir.) Eşzamanlılık işleme için herhangi bir kodlama yapmazsanız, Istemci WINS otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="93595-360">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="93595-361">Can'ın değişiklik DB'de güncelleştirilmesini engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="93595-361">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="93595-362">Genellikle, bir uygulamayı atadığınız:</span><span class="sxs-lookup"><span data-stu-id="93595-362">Typically, the app would:</span></span>

  * <span data-ttu-id="93595-363">Bir hata iletisi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="93595-363">Display an error message.</span></span>
  * <span data-ttu-id="93595-364">Verilerin geçerli durumunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="93595-364">Show the current state of the data.</span></span>
  * <span data-ttu-id="93595-365">Değişiklikleri uygulamak verin.</span><span class="sxs-lookup"><span data-stu-id="93595-365">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="93595-366">Bu adlı bir *Store WINS* senaryo.</span><span class="sxs-lookup"><span data-stu-id="93595-366">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="93595-367">(Veri deposu değerleri, istemci tarafından gönderilen değerlere göre önceliklidir.) Bu öğreticide mağaza WINS senaryosunu uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="93595-367">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="93595-368">Bu yöntem, hiçbir değişiklik uyarı bir kullanıcı kılınmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="93595-368">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="93595-369">Eşzamanlılığı işleme</span><span class="sxs-lookup"><span data-stu-id="93595-369">Handling concurrency</span></span> 

<span data-ttu-id="93595-370">Ne zaman bir özellik olarak yapılandırıldığında bir [eşzamanlılık belirteci](/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="93595-370">When a property is configured as a [concurrency token](/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="93595-371">EF Core getirildi sonra'nın özellik değiştirilmedi doğrular.</span><span class="sxs-lookup"><span data-stu-id="93595-371">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="93595-372">Onay gerçekleşir, [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) veya [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="93595-372">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="93595-373">Özelliği, getirildikten sonra değiştirilmişse, bir [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-373">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="93595-374">DB ve veri modeli oluşturma destekleyecek şekilde yapılandırılması gerekir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="93595-374">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="93595-375">Bir özellik eşzamanlılık çakışmaları algılama</span><span class="sxs-lookup"><span data-stu-id="93595-375">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="93595-376">Eşzamanlılık çakışmaları ile özellik düzeyinde algılanamıyor [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="93595-376">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="93595-377">Öznitelik, birden çok modelin özellikleri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="93595-377">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="93595-378">Daha fazla bilgi için [veri ek açıklamaları-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="93595-378">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="93595-379">`[ConcurrencyCheck]` Özniteliği, bu öğreticide kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="93595-379">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="93595-380">Bir satırda eşzamanlılık çakışmaları algılama</span><span class="sxs-lookup"><span data-stu-id="93595-380">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="93595-381">Eşzamanlılık çakışmalarını algılamak için bir [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) sütun izleme modele eklenir.</span><span class="sxs-lookup"><span data-stu-id="93595-381">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="93595-382">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="93595-382">`rowversion` :</span></span>

* <span data-ttu-id="93595-383">SQL Server özeldir.</span><span class="sxs-lookup"><span data-stu-id="93595-383">Is SQL Server specific.</span></span> <span data-ttu-id="93595-384">Diğer veritabanlarının benzer bir özellik sağlamayabilir.</span><span class="sxs-lookup"><span data-stu-id="93595-384">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="93595-385">Veritabanından getirildikten sonra'nın bir varlık değiştirilmediğinden belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="93595-385">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="93595-386">Bir sıralı bir veritabanı oluşturur `rowversion` her zaman satır artan sayısı güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="93595-386">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="93595-387">İçinde bir `Update` veya `Delete` komutu `Where` yan tümcesi içeren getirilen değeri `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="93595-387">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="93595-388">Güncelleştirilen satır değiştiyse:</span><span class="sxs-lookup"><span data-stu-id="93595-388">If the row being updated has changed:</span></span>

* <span data-ttu-id="93595-389">`rowversion` getirilen değeri ile eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="93595-389">`rowversion` doesn't match the fetched value.</span></span>
* <span data-ttu-id="93595-390">`Update` Veya `Delete` olduğundan, komut satır Bul yok `Where` yan tümcesi içeren getirilen `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="93595-390">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
* <span data-ttu-id="93595-391">A `DbUpdateConcurrencyException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-391">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="93595-392">EF Core tarafından hiçbir satır güncelleştirildiğinde içinde bir `Update` veya `Delete` komutu, bir eşzamanlılık özel durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-392">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="93595-393">Departman varlığa izleme özelliği ekleme</span><span class="sxs-lookup"><span data-stu-id="93595-393">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="93595-394">İçinde *Models/Department.cs*, RowVersion adlı izleme özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="93595-394">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="93595-395">[Zaman damgası](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) özniteliği, bu sütunda yer belirtir `Where` yan tümcesi `Update` ve `Delete` komutları.</span><span class="sxs-lookup"><span data-stu-id="93595-395">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="93595-396">Adlandırılan öznitelik `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` veri türü SQL önce `rowversion` türü değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="93595-396">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="93595-397">Fluent API'si izleme özelliği de belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="93595-397">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="93595-398">Aşağıdaki kod, T-SQL bölüm adını güncelleştirildiğinde EF Core tarafından oluşturulan bir bölümü gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="93595-398">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

<span data-ttu-id="93595-399">Önceki kodun gösterdiği vurgulanmış `WHERE` yan tümcesi içeren `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="93595-399">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="93595-400">Varsa DB `RowVersion` eşit değildir `RowVersion` parametre (`@p2`), satır güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="93595-400">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="93595-401">Aşağıdaki vurgulanmış kodu tam olarak bir satır güncellenmedi doğrular T-SQL gösterir:</span><span class="sxs-lookup"><span data-stu-id="93595-401">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

<span data-ttu-id="93595-402">[@@ROWCOUNT ](/sql/t-sql/functions/rowcount-transact-sql) son deyiminden etkilenen satır sayısını döndürür.</span><span class="sxs-lookup"><span data-stu-id="93595-402">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="93595-403">Hayır, satır güncelleştirilir, EF Core oluşturur bir `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="93595-403">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="93595-404">T-SQL EF Core oluşturur Visual Studio çıktı penceresinde görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-404">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="93595-405">DB update</span><span class="sxs-lookup"><span data-stu-id="93595-405">Update the DB</span></span>

<span data-ttu-id="93595-406">Ekleme `RowVersion` geçiş gerektiren DB modeli özelliğini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="93595-406">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="93595-407">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-407">Build the project.</span></span> <span data-ttu-id="93595-408">Bir komut penceresinde aşağıdakileri girin:</span><span class="sxs-lookup"><span data-stu-id="93595-408">Enter the following in a command window:</span></span>

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="93595-409">Yukarıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="93595-409">The preceding commands:</span></span>

* <span data-ttu-id="93595-410">Ekler *geçişleri / {zaman stamp}_RowVersion.cs* geçiş dosyası.</span><span class="sxs-lookup"><span data-stu-id="93595-410">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="93595-411">Güncelleştirmeleri *Migrations/SchoolContextModelSnapshot.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="93595-411">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="93595-412">Bu güncelleştirme aşağıdaki vurgulanmış kodu ekler `BuildModel` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="93595-412">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* <span data-ttu-id="93595-413">DB güncelleştirilemedi geçişlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="93595-413">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a><span data-ttu-id="93595-414">İskele Departmanlar modeli</span><span class="sxs-lookup"><span data-stu-id="93595-414">Scaffold the Departments model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93595-415">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93595-415">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="93595-416">Bölümündeki yönergeleri [Öğrenci modeli iskelesini](xref:data/ef-rp/intro#scaffold-student-pages) ve `Department` model sınıfı için.</span><span class="sxs-lookup"><span data-stu-id="93595-416">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Department` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93595-417">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93595-417">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="93595-418">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="93595-418">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="93595-419">Önceki komut iskelesini kurar `Department` modeli.</span><span class="sxs-lookup"><span data-stu-id="93595-419">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="93595-420">Projeyi Visual Studio'da açın.</span><span class="sxs-lookup"><span data-stu-id="93595-420">Open the project in Visual Studio.</span></span>

<span data-ttu-id="93595-421">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-421">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="93595-422">Departmanlar dizin sayfası</span><span class="sxs-lookup"><span data-stu-id="93595-422">Update the Departments Index page</span></span>

<span data-ttu-id="93595-423">Oluşturulan yapı iskelesi altyapısı bir `RowVersion` sütunu için dizin sayfasını, ancak bu alanı olmamalıdır görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="93595-423">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="93595-424">Bu öğreticide, son baytı `RowVersion` eşzamanlılık anlamanıza yardımcı olması için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-424">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="93595-425">Son bayta kalan benzersiz olması garanti yoktur.</span><span class="sxs-lookup"><span data-stu-id="93595-425">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="93595-426">Gerçek bir uygulamada görüntüleyemiyordu `RowVersion` veya son baytı `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="93595-426">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="93595-427">Dizin Sayfası güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="93595-427">Update the Index page:</span></span>

* <span data-ttu-id="93595-428">Dizin bölümlerine değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-428">Replace Index with Departments.</span></span>
* <span data-ttu-id="93595-429">Biçimlendirme içeren değiştirin `RowVersion` son baytı ile `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="93595-429">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="93595-430">FirstMidName tam adı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-430">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="93595-431">Güncelleştirilen sayfaya aşağıdaki işaretlemeyi gösterir:</span><span class="sxs-lookup"><span data-stu-id="93595-431">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="93595-432">Düzenleme sayfa modeli güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="93595-432">Update the Edit page model</span></span>

<span data-ttu-id="93595-433">Aşağıdaki kodla *Pages\Departments\Edit.cshtml.cs* güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="93595-433">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="93595-434">Bir eşzamanlılık algılanması [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) güncelleştirilmesi `rowVersion` alınan varlık değeri.</span><span class="sxs-lookup"><span data-stu-id="93595-434">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="93595-435">EF Core özgün içeren bir WHERE yan tümcesi ile SQL güncelleştirme komut oluşturur `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="93595-435">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="93595-436">Hiçbir satır güncelleştirme komutu tarafından etkileniyorsanız (hiçbir satır özgün sahip `RowVersion` değer), `DbUpdateConcurrencyException` özel durumu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-436">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="93595-437">Önceki kodda, `Department.RowVersion` varlık getirildi değeri olur.</span><span class="sxs-lookup"><span data-stu-id="93595-437">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="93595-438">`OriginalValue` DB değer olduğunda `FirstOrDefaultAsync` bu yöntemi çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="93595-438">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="93595-439">Aşağıdaki kod, istemci değerler (Bu yönteme gönderilen değerler) ve DB değerleri alır:</span><span class="sxs-lookup"><span data-stu-id="93595-439">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="93595-440">Aşağıdaki kod, `OnPostAsync`gönderilen değerden farklı olan DB değerleri olan her bir sütun için özel bir hata iletisi ekler:</span><span class="sxs-lookup"><span data-stu-id="93595-440">The following code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="93595-441">Aşağıdaki vurgulanmış kodu kümeleri `RowVersion` değerden yeni değere Veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="93595-441">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="93595-442">Kullanıcının bir sonraki tıklayışında **Kaydet**, düzenleme sayfası son görüntülenmesini yakalandı beri gerçekleşen eşzamanlılık hataları.</span><span class="sxs-lookup"><span data-stu-id="93595-442">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="93595-443">`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski olan `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="93595-443">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="93595-444">Razor Sayfası'nda `ModelState` değerinin ikisi de mevcut olduğunda alan üzerinde model özellik değerlerini öncelik kazanır.</span><span class="sxs-lookup"><span data-stu-id="93595-444">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="93595-445">Güncelleştirme düzenleme sayfası</span><span class="sxs-lookup"><span data-stu-id="93595-445">Update the Edit page</span></span>

<span data-ttu-id="93595-446">Güncelleştirme *Pages/Departments/Edit.cshtml* aşağıdaki işaretlemeyle:</span><span class="sxs-lookup"><span data-stu-id="93595-446">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="93595-447">Önceki işaretlemesi:</span><span class="sxs-lookup"><span data-stu-id="93595-447">The preceding markup:</span></span>

* <span data-ttu-id="93595-448">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="93595-448">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="93595-449">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="93595-449">Adds a hidden row version.</span></span> <span data-ttu-id="93595-450">`RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="93595-450">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="93595-451">Son baytı görüntüler `RowVersion` hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="93595-451">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="93595-452">Değiştirir `ViewData` türü kesin belirlenmiş ile `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="93595-452">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="93595-453">Eşzamanlılık çakışmalarını düzenleme sayfası ile test</span><span class="sxs-lookup"><span data-stu-id="93595-453">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="93595-454">İki tarayıcılar örneklerinde düzenleme İngilizce departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="93595-454">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="93595-455">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="93595-455">Run the app and select Departments.</span></span>
* <span data-ttu-id="93595-456">Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="93595-456">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="93595-457">Birinci sekmede tıklayın **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="93595-457">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="93595-458">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="93595-458">The two browser tabs display the same information.</span></span>

<span data-ttu-id="93595-459">' A tıklayın ve ilk tarayıcı sekmesine adını değiştirmek **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="93595-459">Change the name in the first browser tab and click **Save**.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="93595-461">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="93595-461">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="93595-462">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-462">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="93595-463">İkinci bir tarayıcı sekmesinde farklı bir alana değiştirin.</span><span class="sxs-lookup"><span data-stu-id="93595-463">Change a different field in the second browser tab.</span></span>

![Departman düzenleme değişikliğinden sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="93595-465">**Kaydet**’e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="93595-465">Click **Save**.</span></span> <span data-ttu-id="93595-466">DB değerleri eşleşmeyen tüm alanlar için hata iletileri görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="93595-466">You see error messages for all fields that don't match the DB values:</span></span>

![Departman düzenleme sayfa hata iletisi](concurrency/_static/edit-error.png)

<span data-ttu-id="93595-468">Ad alanı değiştirmek bu tarayıcı penceresini düşünmediğiniz.</span><span class="sxs-lookup"><span data-stu-id="93595-468">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="93595-469">Kopyalama ve geçerli değerin (dil) adı alanına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="93595-469">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="93595-470">Sekme genişletme. İstemci tarafı doğrulaması hata iletisini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="93595-470">Tab out. Client-side validation removes the error message.</span></span>

![Departman düzenleme sayfa hata iletisi](concurrency/_static/cv.png)

<span data-ttu-id="93595-472">Tıklayın **Kaydet** yeniden.</span><span class="sxs-lookup"><span data-stu-id="93595-472">Click **Save** again.</span></span> <span data-ttu-id="93595-473">İkinci tarayıcı sekmesinde girdiğiniz değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="93595-473">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="93595-474">Dizin sayfasında kaydedilen değerler görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="93595-474">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="93595-475">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="93595-475">Update the Delete page</span></span>

<span data-ttu-id="93595-476">Delete sayfa modeli aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="93595-476">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="93595-477">Varlık getirildi sonra değiştiğinde silme sayfası eşzamanlılık çakışmalarını algılar.</span><span class="sxs-lookup"><span data-stu-id="93595-477">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="93595-478">`Department.RowVersion` Varlık getirildi satır sürümü andır.</span><span class="sxs-lookup"><span data-stu-id="93595-478">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="93595-479">EF Core SQL DELETE komutu oluşturduğunda, WHERE yan tümcesi ile içerdiği `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="93595-479">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="93595-480">Sıfır satır SQL DELETE komutu sonuçları etkileniyorsa:</span><span class="sxs-lookup"><span data-stu-id="93595-480">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="93595-481">`RowVersion` SQL DELETE komutu eşleşmiyor `RowVersion` DB'de.</span><span class="sxs-lookup"><span data-stu-id="93595-481">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="93595-482">DbUpdateConcurrencyException özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="93595-482">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="93595-483">`OnGetAsync` çağrılır `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="93595-483">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="93595-484">Silme sayfası</span><span class="sxs-lookup"><span data-stu-id="93595-484">Update the Delete page</span></span>

<span data-ttu-id="93595-485">Güncelleştirme *Pages/Departments/Delete.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="93595-485">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="93595-486">Yukarıdaki kod aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="93595-486">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="93595-487">Güncelleştirmeleri `page` gelen yönerge `@page` için `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="93595-487">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="93595-488">Bir hata iletisi ekler.</span><span class="sxs-lookup"><span data-stu-id="93595-488">Adds an error message.</span></span>
* <span data-ttu-id="93595-489">FullName FirstMidName değiştirir **yönetici** alan.</span><span class="sxs-lookup"><span data-stu-id="93595-489">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="93595-490">Değişiklikleri `RowVersion` son bayt görüntülenecek.</span><span class="sxs-lookup"><span data-stu-id="93595-490">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="93595-491">Gizli satır sürümü ekler.</span><span class="sxs-lookup"><span data-stu-id="93595-491">Adds a hidden row version.</span></span> <span data-ttu-id="93595-492">`RowVersion` POST geri değere bağlar, böylece eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="93595-492">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="93595-493">Eşzamanlılık çakışmalarını silme sayfası ile test</span><span class="sxs-lookup"><span data-stu-id="93595-493">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="93595-494">Test bölümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="93595-494">Create a test department.</span></span>

<span data-ttu-id="93595-495">İki tarayıcılar örneklerinde DELETE test departmanı açın:</span><span class="sxs-lookup"><span data-stu-id="93595-495">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="93595-496">Uygulamayı çalıştırın ve bölümleri seçin.</span><span class="sxs-lookup"><span data-stu-id="93595-496">Run the app and select Departments.</span></span>
* <span data-ttu-id="93595-497">Sağ **Sil** seçin ve test departmanı için köprü **yeni sekmede aç**.</span><span class="sxs-lookup"><span data-stu-id="93595-497">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="93595-498">Tıklayın **Düzenle** köprü test bölümü için.</span><span class="sxs-lookup"><span data-stu-id="93595-498">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="93595-499">Aynı bilgilerin iki tarayıcı sekmeleri görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="93595-499">The two browser tabs display the same information.</span></span>

<span data-ttu-id="93595-500">İlk tarayıcı sekmesine bütçede değiştirip'ı **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="93595-500">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="93595-501">Tarayıcı değiştirilen değer ve güncelleştirilmiş rowVersion göstergesi ile dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="93595-501">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="93595-502">Güncelleştirilmiş rowVersion göstergesi, Not, ikinci geri göndermenin diğer sekmesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-502">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="93595-503">İkinci sekmeden test departmanını silin. Bir eşzamanlılık hatası, VERITABANıNDAKI geçerli değerlerle birlikte görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="93595-503">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="93595-504">Tıklayarak **Sil** sürece varlığını siler `RowVersion` updated.department silinmiş olmuştur.</span><span class="sxs-lookup"><span data-stu-id="93595-504">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="93595-505">Bkz: [devralma](xref:data/ef-mvc/inheritance) nasıl bir veri modeli devralır.</span><span class="sxs-lookup"><span data-stu-id="93595-505">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="93595-506">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="93595-506">Additional resources</span></span>

* [<span data-ttu-id="93595-507">EF Core eşzamanlılık belirteçleri</span><span class="sxs-lookup"><span data-stu-id="93595-507">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="93595-508">EF Core eşzamanlılık işleme</span><span class="sxs-lookup"><span data-stu-id="93595-508">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="93595-509">Bu öğreticinin YouTube sürümü (eşzamanlılık çakışmalarını Işleme)</span><span class="sxs-lookup"><span data-stu-id="93595-509">YouTube version of this tutorial(Handling Concurrency Conflicts)</span></span>](https://youtu.be/EosxHTFgYps)
* [<span data-ttu-id="93595-510">Bu öğreticinin YouTube sürümü (Bölüm 2)</span><span class="sxs-lookup"><span data-stu-id="93595-510">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [<span data-ttu-id="93595-511">Bu öğreticinin YouTube sürümü (Bölüm 3)</span><span class="sxs-lookup"><span data-stu-id="93595-511">YouTube version of this tutorial(Part 3)</span></span>](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [<span data-ttu-id="93595-512">Önceki</span><span class="sxs-lookup"><span data-stu-id="93595-512">Previous</span></span>](xref:data/ef-rp/update-related-data)

::: moniker-end

