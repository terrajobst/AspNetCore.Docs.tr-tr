---
title: "EF çekirdek - eşzamanlılık - 8, 10 ile ASP.NET Core MVC"
author: tdykstra
description: "Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir."
keywords: "ASP.NET Core, Entity Framework Çekirdek eşzamanlılık"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 15e79e15-bda5-441d-80c7-8032a2628605
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: ffe8ef968d7bde9755d5c55389f6f1548f03ffec
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a><span data-ttu-id="f54dc-104">Eşzamanlılık çakışmalarını - EF çekirdek ASP.NET Core MVC Öğreticisi (8, 10) ile işleme</span><span class="sxs-lookup"><span data-stu-id="f54dc-104">Handling concurrency conflicts - EF Core with ASP.NET Core MVC tutorial (8 of 10)</span></span>

<span data-ttu-id="f54dc-105">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f54dc-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f54dc-106">Contoso University örnek web uygulaması Entity Framework Çekirdek ve Visual Studio kullanarak ASP.NET Core MVC web uygulamalarının nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="f54dc-107">Eğitmen serisi hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](intro.md).</span><span class="sxs-lookup"><span data-stu-id="f54dc-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="f54dc-108">Önceki eğitimlerine verileri güncelleştirmek öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-108">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="f54dc-109">Bu öğretici, birden çok kullanıcı aynı anda aynı varlık güncelleştirdiğinizde çakışmalarına gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-109">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="f54dc-110">Departman varlıkla çalışan ve eşzamanlılık hataları işlemek web sayfaları oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="f54dc-110">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="f54dc-111">Aşağıdaki çizimler bir eşzamanlılık çakışması olursa, görüntülenen bazı iletiler de dahil olmak üzere düzenleme ve silme sayfalarında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-111">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Departman Düzenle sayfası](concurrency/_static/edit-error.png)

![Bölüm silme sayfası](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="f54dc-114">Eşzamanlılık çakışmaları</span><span class="sxs-lookup"><span data-stu-id="f54dc-114">Concurrency conflicts</span></span>

<span data-ttu-id="f54dc-115">Bir eşzamanlılık çakışması düzenlemek için bir kullanıcı bir varlığın verileri görüntüler ve ilk kullanıcının değişiklik veritabanına yazılmadan önce başka bir kullanıcı aynı varlığın veri güncelleştirmeleri oluşur.</span><span class="sxs-lookup"><span data-stu-id="f54dc-115">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="f54dc-116">Bu gibi çakışmaları algılama etkinleştirmezseniz aktaranın veritabanını güncelleştiren son diğer kullanıcının değişiklikleri geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-116">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="f54dc-117">Birçok uygulamada, bu riski kabul edilebilir: birkaç kullanıcı ya da birkaç güncelleştirmeleri varsa veya bazı değişiklikleri üzerine yazılmışsa eşzamanlılık programlama maliyetini ağır avantajı gerçekten önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-117">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="f54dc-118">Bu durumda, eşzamanlılık çakışmalarına için uygulamayı yapılandırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f54dc-118">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="f54dc-119">Eşzamanlılık (kilitleme)</span><span class="sxs-lookup"><span data-stu-id="f54dc-119">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="f54dc-120">Uygulamanızı eşzamanlılık senaryolarda yanlışlıkla veri kaybını önlemek gerekiyorsa, bunu yapmanın bir yolu veritabanı kilitleri kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-120">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="f54dc-121">Bu eşzamanlılık çağrılır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-121">This is called pessimistic concurrency.</span></span> <span data-ttu-id="f54dc-122">Örneğin, bir veritabanından bir satır okumadan önce için bir kilit isteği salt okunur veya güncelleştirme erişim.</span><span class="sxs-lookup"><span data-stu-id="f54dc-122">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="f54dc-123">Güncelleştirme erişim için bir satır kilitlemek, başka hiçbir kullanıcı için ya da satır kilitlemek için izin verilir salt okunur veya değiştirilen sürecinde olan verilerin bir kopyasını elde edecekleri çünkü erişimi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-123">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="f54dc-124">Salt okunur erişim için bir satır kilitlemek, diğerleri de salt okunur erişim için kullanılabilir ancak güncelleştirme kilitleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-124">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="f54dc-125">Kilitleri yönetmek dezavantajları vardır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-125">Managing locks has disadvantages.</span></span> <span data-ttu-id="f54dc-126">Programa karmaşık olabilir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-126">It can be complex to program.</span></span> <span data-ttu-id="f54dc-127">Önemli Veritabanı Yönetimi kaynaklarını gerektirir ve bu kullanıcı bir uygulamanın sayısı olarak performans sorunlarına neden olabilir artırır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-127">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="f54dc-128">Bu nedenlerle, tüm veritabanı yönetim sistemleri eşzamanlılık destekler.</span><span class="sxs-lookup"><span data-stu-id="f54dc-128">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="f54dc-129">Entity Framework Çekirdek, yerleşik desteği sağlar ve bu öğreticiyi uyguladıktan nasıl göstermez.</span><span class="sxs-lookup"><span data-stu-id="f54dc-129">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="f54dc-130">İyimser eşzamanlılık</span><span class="sxs-lookup"><span data-stu-id="f54dc-130">Optimistic Concurrency</span></span>

<span data-ttu-id="f54dc-131">Eşzamanlılık iyimser eşzamanlılık alternatifidir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-131">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="f54dc-132">İyimser eşzamanlılık gerçekleşecek şekilde eşzamanlılık çakışması izin vererek ve içermiyorlarsa uygun şekilde tepki anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-132">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="f54dc-133">Örneğin, Jane departmanı düzenleme sayfasını ziyaret ve İngilizce departmanı bütçe tutarını 350,000.00 0,00 için değiştirir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-133">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![Bütçe 0 olarak değiştirme](concurrency/_static/change-budget.png)

<span data-ttu-id="f54dc-135">Jane tıklar önce **kaydetmek**, John aynı sayfasını ziyaret ve başlangıç tarihi alanı 9/1/2007'den 9/1/2013'e değiştirir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-135">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Başlangıç tarihi 2013 için değiştirme](concurrency/_static/change-date.png)

<span data-ttu-id="f54dc-137">Jane tıklar **kaydetmek** ilk ve her tarayıcı dizin sayfasına geri döndüğünde değiştirmek görür.</span><span class="sxs-lookup"><span data-stu-id="f54dc-137">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![Bütçe sıfır olarak değiştirildi](concurrency/_static/budget-zero.png)

<span data-ttu-id="f54dc-139">John tıklattığında **kaydetmek** bir düzenleme sayfasında $350,000.00 bütçe görüntülenmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="f54dc-139">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="f54dc-140">Ne olacağını eşzamanlılık çakışmaları nasıl işleneceğini tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-140">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="f54dc-141">Bazı seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f54dc-141">Some of the options include the following:</span></span>

* <span data-ttu-id="f54dc-142">Bir kullanıcı değiştirdi hangi özelliğinin izlemek ve yalnızca karşılık gelen sütunlara veritabanındaki güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-142">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="f54dc-143">Farklı özellikler iki kullanıcı tarafından güncelleştirildi olduğundan örnek senaryoda, hiçbir veri kaybı, olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-143">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="f54dc-144">Birisi İngilizce departmanı gözatar başlatıldığında bunlar Jane'nın ve Can'ın değişiklikleri--1/9/2013 başlangıç tarihi ve bütçe sıfır Doları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-144">The next time someone browses the English department, they'll see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="f54dc-145">Güncelleştirme bu yöntem veri kaybına sebep çakışmaları sayısını azaltır, ancak bir varlık aynı özelliğe rakip bir değişiklik yaptıysanız, veri kaybını önlemek olamaz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-145">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="f54dc-146">Entity Framework bu şekilde çalışıp çalışmadığını güncelleştirme kodunuzu nasıl uygulayacağınıza bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-146">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="f54dc-147">Bir varlık için tüm özgün özellik değerlerini yanı sıra yeni değerleri izlemek için büyük miktarlarda durumu bakımını gerektirebilir bir web uygulamasında pratik değildir, çünkü genellikle.</span><span class="sxs-lookup"><span data-stu-id="f54dc-147">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="f54dc-148">Büyük miktarlarda durumu koruma uygulama performansı etkileyebilir sunucu kaynakları gerekir ya da web sayfasındaki kendisini (örneğin, gizli alanlar) dahil gerekir çünkü ya da bir tanımlama bilgisinde.</span><span class="sxs-lookup"><span data-stu-id="f54dc-148">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="f54dc-149">Jane'nın değişiklik üzerine Can'ın değişiklik izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-149">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="f54dc-150">Sonraki birisi İngilizce departmanı gözatar, 1/9/2013 görürsünüz ve geri yüklenen $350,000.00 değeri.</span><span class="sxs-lookup"><span data-stu-id="f54dc-150">The next time someone browses the English department, they'll see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="f54dc-151">Bu adlı bir *istemci WINS* veya *WINS'de son* senaryo.</span><span class="sxs-lookup"><span data-stu-id="f54dc-151">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="f54dc-152">(İstemci tüm değerleri veri deposunda nedir üzerinden önceliklidir.) Bu bölüm için giriş hiçbir eşzamanlılık işleme için kodlama yapmazsanız belirtildiği gibi bu otomatik olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-152">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="f54dc-153">Veritabanında güncelleştirilen Can'ın değişiklik engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-153">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="f54dc-154">Genellikle, bir hata iletisi görüntüler ona verilerin geçerli durumunu gösterir ve ona kendisinin bunları olmak istiyorsa, bu değişiklikleri uygulamak izin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-154">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="f54dc-155">Bu adlı bir *deposu WINS* senaryo.</span><span class="sxs-lookup"><span data-stu-id="f54dc-155">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="f54dc-156">(Veri deposu değerlerini istemci tarafından gönderilen değerler önceliklidir.) Bu öğreticide deposu WINS senaryo uygulama.</span><span class="sxs-lookup"><span data-stu-id="f54dc-156">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="f54dc-157">Bu yöntem, hiçbir değişiklik olanlar için uyarı bir kullanıcı olmaksızın üzerine yazılır sağlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-157">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="f54dc-158">Eşzamanlılık çakışmalarını algılama</span><span class="sxs-lookup"><span data-stu-id="f54dc-158">Detecting concurrency conflicts</span></span>

<span data-ttu-id="f54dc-159">İşleyerek çakışmalarını çözme `DbConcurrencyException` Entity Framework oluşturur özel durumları.</span><span class="sxs-lookup"><span data-stu-id="f54dc-159">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="f54dc-160">Bu özel durumlar oluşturma zamanı bilmek için Entity Framework çakışmaları algılayabilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-160">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="f54dc-161">Bu nedenle, veritabanı ve veri modelinin uygun şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-161">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="f54dc-162">Çakışma algılamasını etkinleştirmek için bazı seçenekler aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="f54dc-162">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="f54dc-163">Veritabanı tablosunda bir satırı değiştiğinde belirlemek için kullanılan bir izleme sütun içerir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-163">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="f54dc-164">Daha sonra bu sütun nerede dahil etmek için Entity Framework yapılandırabilirsiniz SQL güncelleştirme veya silme komutlarını yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-164">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="f54dc-165">İzleme sütunun veri türünü genellikle `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="f54dc-165">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="f54dc-166">`rowversion` Satır güncelleştirilir her zaman artar bir sıralı sayı bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-166">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="f54dc-167">Bir güncelleştirme veya silme komutunda Where yan tümcesi izleme sütunu (özgün satır sürümü) özgün değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-167">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="f54dc-168">Güncelleştirilen satır başka bir kullanıcı tarafından değeri değiştirildiğinde, `rowversion` Update veya Delete deyiminde Where nedeniyle güncelleştirilecek satır bulmanız sütun özgün değerinden farklı yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-168">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="f54dc-169">(Diğer bir deyişle, etkilenen satırların sayısı sıfır olduğunda) güncelleştirme veya silme tarafından hiçbir satırın güncelleştirilmediği Entity Framework bulur komutu tıklattığınızda, bir eşzamanlılık çakışması yorumlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-169">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="f54dc-170">Where tablodaki her sütunun özgün değerler eklemek için Entity Framework yapılandırma Update ve Delete komutları yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-170">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="f54dc-171">İlk seçenek satırın ilk okunuşundan bu yana, sıradaki herhangi bir şey değiştiyse, olduğu gibi Where yan tümcesi güncelleştirmek için bir satır dönüş kalmaz, Entity Framework bir eşzamanlılık çakışması yorumlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-171">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="f54dc-172">Çok sayıda sütuna sahip veritabanı tabloları için bu yaklaşım çok büyük Where sonuçlanabilir yan tümceleri ve büyük miktarlarda durumu bakımını gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-172">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="f54dc-173">Daha önce belirtildiği gibi büyük miktarlarda durumu koruma uygulama performansını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-173">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="f54dc-174">Bu nedenle bu yaklaşım genellikle önerilmez ve Bu öğreticide kullanılan yöntem değildir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-174">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="f54dc-175">Eşzamanlılık bu yaklaşımı uygulamak istiyorsanız, tüm birincil anahtar özellikleri'nde ekleyerek eşzamanlılık için izlemek istediğiniz varlık işaretleyin zorunda `ConcurrencyCheck` onlara özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f54dc-175">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="f54dc-176">Bu değişiklik, tüm sütunlar SQL Where yan tümcesinde güncelleştirme ve silme deyimleri dahil etmek Entity Framework sağlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-176">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="f54dc-177">Bu öğreticinin geri kalanında, ekleyeceksiniz bir `rowversion` özelliği departmanı varlığa izleme, bir denetleyici ve görünümler oluşturma ve her şeyin doğru şekilde çalıştığını doğrulamak için test.</span><span class="sxs-lookup"><span data-stu-id="f54dc-177">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="f54dc-178">Departman varlığa izleme özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="f54dc-178">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="f54dc-179">İçinde *Models/Department.cs*, RowVersion adlı bir izleme özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="f54dc-179">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="f54dc-180">`Timestamp` Özniteliği belirtir. Bu sütun eklenecek Where yan tümcesi Update ve Delete komutların veritabanına gönderilen.</span><span class="sxs-lookup"><span data-stu-id="f54dc-180">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="f54dc-181">Öznitelik adı verilen `Timestamp` bir SQL SQL Server'ın önceki sürümlerinde kullanılan çünkü `timestamp` önce SQL veri türü `rowversion` yerine.</span><span class="sxs-lookup"><span data-stu-id="f54dc-181">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="f54dc-182">.NET türü için `rowversion` bir bayt dizisi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-182">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="f54dc-183">Fluent API kullanmayı tercih ederseniz kullanabilirsiniz `IsConcurrencyToken` yöntemi (içinde *Data/SchoolContext.cs*) aşağıdaki örnekte gösterildiği gibi izleme özelliği belirtmek için:</span><span class="sxs-lookup"><span data-stu-id="f54dc-183">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="f54dc-184">Başka bir geçiş gerçekleştirmeniz gereken şekilde bir özelliği ekleyerek veritabanı modeli değişti.</span><span class="sxs-lookup"><span data-stu-id="f54dc-184">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="f54dc-185">Değişikliklerinizi kaydetmek ve projeyi oluşturun ve ardından komut penceresinde aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="f54dc-185">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="f54dc-186">Departmanlar denetleyicisi ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="f54dc-186">Create a Departments controller and views</span></span>

<span data-ttu-id="f54dc-187">Öğrenciler, kurslar ve Eğitmen için daha önce yaptığınız gibi bir Departmanlar denetleyici ve görünüm iskelesi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-187">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![İskele bölüm](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="f54dc-189">İçinde *DepartmentsController.cs* dosya, departman yönetici açılan listeleri Eğitmen tam adı yerine yalnızca son adını içerecek şekilde "FirstMidName" dört tüm oluşumlarını "FullName" değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-189">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="f54dc-190">Departmanlar dizini görünümünü güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="f54dc-190">Update the Departments Index view</span></span>

<span data-ttu-id="f54dc-191">Yapı iskelesi altyapısı RowVersion sütun dizini görünümde oluşturulmuş, ancak bu alan görüntülenen döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-191">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="f54dc-192">Kodla *Views/Departments/Index.cshtml* aşağıdaki kod ile.</span><span class="sxs-lookup"><span data-stu-id="f54dc-192">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="f54dc-193">Bu "Departman" başlığı değişikliklerini RowVersion sütunu siler ve yöneticisinin adı yerine tam adını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-193">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="f54dc-194">Güncelleştirme Departmanlar denetleyicideki düzenleme yöntemleri</span><span class="sxs-lookup"><span data-stu-id="f54dc-194">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="f54dc-195">Her iki HttpGet içinde `Edit` yöntemi ve `Details` yöntemi Ekle `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="f54dc-195">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="f54dc-196">HttpGet içinde `Edit` yöntemi, istekli yükleme için yönetici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-196">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="f54dc-197">HttpPost için var olan kodu `Edit` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f54dc-197">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="f54dc-198">Kod bölümünün güncelleştirilmesi okumaya çalışırken tarafından başlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-198">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="f54dc-199">Varsa `SingleOrDefaultAsync` yöntemi null değerini döndürür, departman başka bir kullanıcı tarafından silindi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-199">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="f54dc-200">Bu durumda kod düzenleme sayfasını bir hata iletisi ile görünürler bir bölüm varlık oluşturmak üzere gönderilen form değerleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-200">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="f54dc-201">Alternatif olarak, departman alanları yeniden görüntüleme olmadan yalnızca bir hata iletisi görüntülerseniz departmanı varlık yeniden oluşturmak zorunda olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-201">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="f54dc-202">Özgün görünümü depolar `RowVersion` değere gizli bir alan ve bu yöntem alan değeri `rowVersion` parametresi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-202">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="f54dc-203">Çağırmadan önce `SaveChanges`, özgün put zorunda `RowVersion` özellik değeri `OriginalValues` varlık koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="f54dc-203">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="f54dc-204">Entity Framework SQL güncelleştirme komut oluşturduğunda, bu komut için özgün sahip bir satır benzeyen bir WHERE yan tümcesi içerecektir sonra `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="f54dc-204">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="f54dc-205">Hiçbir satır güncelleştirme komutu tarafından etkilenen varsa (özgün hiçbir satır sahip `RowVersion` değeri), Entity Framework oluşturur bir `DbUpdateConcurrencyException` özel durum.</span><span class="sxs-lookup"><span data-stu-id="f54dc-205">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="f54dc-206">Başka bir özel durum için catch bloğu kodda güncelleştirilmiş değerleri sahip etkilenen departman varlığı alır `Entries` özel durum nesnesi özelliği.</span><span class="sxs-lookup"><span data-stu-id="f54dc-206">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="f54dc-207">`Entries` Koleksiyonu tek olacaktır `EntityEntry` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-207">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="f54dc-208">Kullanıcı tarafından girilen yeni ve geçerli veritabanı değerlerini almak için bu nesneyi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-208">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="f54dc-209">Kod Düzenle iletişim kutusunda girilen kullanıcı sayfasında veritabanı değerleri farklı olan her bir sütun için bir özel hata iletisi ekler (yalnızca bir alan burada gösterilmiştir okumanızdır).</span><span class="sxs-lookup"><span data-stu-id="f54dc-209">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="f54dc-210">Son olarak, kod ayarlar `RowVersion` değerini `departmentToUpdate` yeni değere veritabanından alınır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-210">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="f54dc-211">Bu yeni `RowVersion` değeri sayfa yeniden düzenleme ve sonraki kullanıcı çalıştırdığında gizli alanında depolanacağı **kaydetmek**, düzenleme sayfasını yeniden yakalanan bu yana, eşzamanlılık hataları.</span><span class="sxs-lookup"><span data-stu-id="f54dc-211">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="f54dc-212">`ModelState.Remove` Deyimi, çünkü gereklidir `ModelState` eski sahip `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="f54dc-212">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="f54dc-213">Görünümünde `ModelState` her ikisi de mevcut olduğunda bir alan modeli özellik değerlerini önceliklidir için bir değer.</span><span class="sxs-lookup"><span data-stu-id="f54dc-213">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="f54dc-214">Güncelleştirme görünümü departmanı Düzenle</span><span class="sxs-lookup"><span data-stu-id="f54dc-214">Update the Department Edit view</span></span>

<span data-ttu-id="f54dc-215">İçinde *Views/Departments/Edit.cshtml*, aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="f54dc-215">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="f54dc-216">Kaydetmek için gizli bir alan eklemek `RowVersion` hemen gizli alanı için aşağıdaki özellik değeri `DepartmentID` özelliği.</span><span class="sxs-lookup"><span data-stu-id="f54dc-216">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="f54dc-217">Bir "Yönetici seçin" seçeneği, aşağı açılan listesine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-217">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="f54dc-218">Düzenleme sayfasını test eşzamanlılık çakışıyor</span><span class="sxs-lookup"><span data-stu-id="f54dc-218">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="f54dc-219">Uygulamayı çalıştırın ve Departmanlar dizin sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-219">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="f54dc-220">Sağ **Düzenle** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**, ardından **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="f54dc-220">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="f54dc-221">İki tarayıcı sekmeleri artık aynı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="f54dc-221">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="f54dc-222">İlk tarayıcı sekmesinde alanında değiştirip'ı **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="f54dc-222">Change a field in the first browser tab and click **Save**.</span></span>

![Departman düzenleme değişiklikten sonra sayfa 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="f54dc-224">Tarayıcı değiştirilen değerle dizin sayfası gösterilir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-224">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="f54dc-225">İkinci bir tarayıcı sekmesinde alanında değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-225">Change a field in the second browser tab.</span></span>

![Departman düzenleme değişiklikten sonra sayfa 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="f54dc-227">**Kaydet**'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="f54dc-227">Click **Save**.</span></span> <span data-ttu-id="f54dc-228">Bir hata iletisi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="f54dc-228">You see an error message:</span></span>

![Departman Düzenle sayfası hata iletisi](concurrency/_static/edit-error.png)

<span data-ttu-id="f54dc-230">Tıklatın **kaydetmek** yeniden.</span><span class="sxs-lookup"><span data-stu-id="f54dc-230">Click **Save** again.</span></span> <span data-ttu-id="f54dc-231">İkinci bir tarayıcı sekmesinde girdiğiniz değer kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-231">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="f54dc-232">Dizin Sayfası göründüğünde, kaydedilen değerler görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-232">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="f54dc-233">Güncelleştirme Sil sayfası</span><span class="sxs-lookup"><span data-stu-id="f54dc-233">Update the Delete page</span></span>

<span data-ttu-id="f54dc-234">Delete sayfa için Entity Framework birisi başka departman benzer bir şekilde düzenleyerek nedeni eşzamanlılık çakışması algılar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-234">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="f54dc-235">Zaman HttpGet `Delete` yöntemi onay görünümü görüntüler, özgün görünümü içerir `RowVersion` gizli bir alan değeri.</span><span class="sxs-lookup"><span data-stu-id="f54dc-235">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="f54dc-236">Değer sonra HttpPost için kullanılabilir olduğunu `Delete` kullanıcı silme doğruladığında çağrılan yöntem.</span><span class="sxs-lookup"><span data-stu-id="f54dc-236">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="f54dc-237">Entity Framework SQL DELETE komutu oluşturduğunda, özgün WHERE yan tümcesi içeren `RowVersion` değeri.</span><span class="sxs-lookup"><span data-stu-id="f54dc-237">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="f54dc-238">Sıfır satır komutu sonuçlarında (satır silme onayı sayfası görüntülendikten sonra değiştirildiği anlamına gelir) etkilenen, bir eşzamanlılık özel durum oluşur ve HttpGet `Delete` yöntemi görüntülemek için true olarak hata bayrak kümesi ile çağrılır onay sayfasında bir hata iletisi ile.</span><span class="sxs-lookup"><span data-stu-id="f54dc-238">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="f54dc-239">Bu durumda, hata iletisi görüntülenir şekilde satır başka bir kullanıcı tarafından silindiği için sıfır satır etkilendi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f54dc-239">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="f54dc-240">Departmanlar denetleyicideki Sil yöntemlerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="f54dc-240">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="f54dc-241">İçinde *DepartmentsController.cs*, HttpGet Değiştir `Delete` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f54dc-241">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="f54dc-242">Yöntemi, bir eşzamanlılık hatası sonra sayfanın görünürler olup olmadığını belirten isteğe bağlı bir parametre kabul eder.</span><span class="sxs-lookup"><span data-stu-id="f54dc-242">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="f54dc-243">Bu bayrağı true olarak ayarlandığında ve artık departman mevcut değilse, başka bir kullanıcı tarafından silindi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-243">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="f54dc-244">Bu durumda, kod dizin sayfasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-244">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="f54dc-245">Bu bayrağı true olarak ayarlandığında ve departman mevcut değilse, başka bir kullanıcı tarafından değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="f54dc-245">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="f54dc-246">Bu durumda, görünümü kullanarak kodu bir hata iletisi gönderir `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="f54dc-246">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="f54dc-247">HttpPost kodla `Delete` yöntemi (adlı `DeleteConfirmed`) aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="f54dc-247">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="f54dc-248">Yalnızca değiştirilen kurulmuş kodda, bu yöntem yalnızca bir kayıt kimliği kabul:</span><span class="sxs-lookup"><span data-stu-id="f54dc-248">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="f54dc-249">Model bağlayıcı tarafından oluşturulan bir bölüm varlık örneği için bu parametreyi değiştirdiniz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-249">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="f54dc-250">Bu kayıt anahtarı yanı sıra RowVersion özellik değerine EF erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-250">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="f54dc-251">Ayrıca eylem yönteminin adı değiştirilmiştir `DeleteConfirmed` için `Delete`.</span><span class="sxs-lookup"><span data-stu-id="f54dc-251">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="f54dc-252">İskele kurulmuş kod kullanılan adı `DeleteConfirmed` HttpPost yöntemi benzersiz bir imza vermek için.</span><span class="sxs-lookup"><span data-stu-id="f54dc-252">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="f54dc-253">(CLR farklı yöntem parametreleri sağlamak için aşırı yüklenmiş yöntemler gerektirir.) İmzaları benzersiz, MVC kuralı ile takılıyor ve HttpPost ve HttpGet silme yöntemleri için aynı adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="f54dc-253">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="f54dc-254">Departman zaten silinmişse `AnyAsync` yöntemi false döndürür ve uygulamanın yalnızca geri dizin yönteme girer.</span><span class="sxs-lookup"><span data-stu-id="f54dc-254">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="f54dc-255">Bir eşzamanlılık hatası yakalanmışsa kodu silme onayı sayfası görüntüler ve bunu belirten bir bayrak bir eşzamanlılık hata iletisi görüntülenmelidir sağlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-255">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="f54dc-256">Delete görünümünü güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="f54dc-256">Update the Delete view</span></span>

<span data-ttu-id="f54dc-257">İçinde *Views/Departments/Delete.cshtml*, kurulmuş kodu bir hata iletisini alan ve Gizli alanlar DepartmentID ve RowVersion özellikleri için ekleyen aşağıdaki kod ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-257">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="f54dc-258">Değişiklikler vurgulanır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-258">The changes are highlighted.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="f54dc-259">Aşağıdaki değişiklikleri yapar:</span><span class="sxs-lookup"><span data-stu-id="f54dc-259">This makes the following changes:</span></span>

* <span data-ttu-id="f54dc-260">Arasında bir hata iletisi ekler `h2` ve `h3` başlıkları.</span><span class="sxs-lookup"><span data-stu-id="f54dc-260">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="f54dc-261">FullName içinde FirstMidName değiştirir **yönetici** alan.</span><span class="sxs-lookup"><span data-stu-id="f54dc-261">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="f54dc-262">RowVersion alan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="f54dc-262">Removes the RowVersion field.</span></span>

* <span data-ttu-id="f54dc-263">İçin gizli bir alan ekler `RowVersion` özelliği.</span><span class="sxs-lookup"><span data-stu-id="f54dc-263">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="f54dc-264">Uygulamayı çalıştırın ve Departmanlar dizin sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="f54dc-264">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="f54dc-265">Sağ **silmek** seçin ve İngilizce departmanı için köprü **yeni sekmede aç**, ardından ilk sekmesini tıklatın **Düzenle** İngilizce departmanı için köprü.</span><span class="sxs-lookup"><span data-stu-id="f54dc-265">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="f54dc-266">İlk penceresinde değerlerden birini değiştirmek ve tıklatın **kaydetmek**:</span><span class="sxs-lookup"><span data-stu-id="f54dc-266">In the first window, change one of the values, and click **Save**:</span></span>

![Delete önce değişiklikten sonra departman Düzenle sayfası](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="f54dc-268">İkinci sekmesini tıklatın **silmek**.</span><span class="sxs-lookup"><span data-stu-id="f54dc-268">In the second tab, click **Delete**.</span></span> <span data-ttu-id="f54dc-269">Eşzamanlılık hata iletisine bakın ve departman değerleri şu anda veritabanında nedir ile yenilenir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-269">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Bölüm silme onayı sayfası eşzamanlılık hatası](concurrency/_static/delete-error.png)

<span data-ttu-id="f54dc-271">Tıklatırsanız **silmek** yeniden departman silinip silinmediğini gösterir dizin sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-271">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="f54dc-272">Güncelleştirme ayrıntıları ve görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="f54dc-272">Update Details and Create views</span></span>

<span data-ttu-id="f54dc-273">İsteğe bağlı olarak kurulmuş kodu ayrıntıları temizlemek ve görünümler oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f54dc-273">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="f54dc-274">Kodla *Views/Departments/Details.cshtml* RowVersion sütun silip yönetici tam adını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-274">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="f54dc-275">Kodla *Views/Departments/Create.cshtml* Seç seçeneği aşağı açılan listeye eklemek için.</span><span class="sxs-lookup"><span data-stu-id="f54dc-275">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="f54dc-276">Özet</span><span class="sxs-lookup"><span data-stu-id="f54dc-276">Summary</span></span>

<span data-ttu-id="f54dc-277">Bu, eşzamanlılık çakışmalarını işleme giriş tamamlar.</span><span class="sxs-lookup"><span data-stu-id="f54dc-277">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="f54dc-278">EF çekirdek eşzamanlılık nasıl ele alınacağını hakkında daha fazla bilgi için bkz: [eşzamanlılık çakışması](https://docs.microsoft.com/ef/core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="f54dc-278">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="f54dc-279">Sonraki öğretici nasıl Eğitmen ve Öğrenci varlıklar için tablo başına hiyerarşisi devralma uygulandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f54dc-279">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f54dc-280">[Önceki](update-related-data.md)
[sonraki](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="f54dc-280">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  