---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: "Bir veritabanı oluşturun | Microsoft Docs"
author: microsoft
description: "2. adım Yemeği tümünün bulunduran bir veritabanı oluşturun ve veri NerdDinner uygulamamız için RSVP adımlarını gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a><span data-ttu-id="80ead-103">Bir veritabanı oluşturun</span><span class="sxs-lookup"><span data-stu-id="80ead-103">Create a Database</span></span>
====================
<span data-ttu-id="80ead-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="80ead-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="80ead-105">PDF indirin</span><span class="sxs-lookup"><span data-stu-id="80ead-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="80ead-106">Adım 2 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="80ead-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="80ead-107">2. adım Yemeği tümünün bulunduran bir veritabanı oluşturun ve veri NerdDinner uygulamamız için RSVP adımlarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="80ead-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="80ead-108">ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.</span><span class="sxs-lookup"><span data-stu-id="80ead-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="80ead-109">NerdDinner adım 2: veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="80ead-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="80ead-110">Biz bir veritabanı NerdDinner uygulamamız için tüm Yemeği ve RSVP verileri depolamak için kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="80ead-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="80ead-111">Ücretsiz SQL Server Express edition'ı kullanarak veritabanı oluşturma adımları Göster (hangi V2 birini kullanarak kolayca yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="80ead-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="80ead-112">SQL Server Express ve tam SQL Server ile tüm yazma kod çalışır.</span><span class="sxs-lookup"><span data-stu-id="80ead-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="80ead-113">Yeni bir SQL Server Express veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="80ead-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="80ead-114">Biz bizim web projeye sağ tıklayarak başlayın ve ardından **Ekle -&gt;yeni öğe** menü komutu:</span><span class="sxs-lookup"><span data-stu-id="80ead-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="80ead-115">Bu, Visual Studio'nun "Yeni Öğe Ekle" iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="80ead-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="80ead-116">Biz "Data" kategoriye göre filtre ve "SQL Server veritabanı" öğe şablonu seçin:</span><span class="sxs-lookup"><span data-stu-id="80ead-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="80ead-117">Biz "NerdDinner.mdf" oluşturabilir ve Tamam isabet istiyoruz SQL Server Express veritabanı adı.</span><span class="sxs-lookup"><span data-stu-id="80ead-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="80ead-118">Visual Studio sonra bize Biz bu dosya için bizim \App eklemek isteyip istemediğinizi sorar\_veri dizini (bir dizin zaten olduğu kurulum ile hem de okuma ve güvenlik ACL yazma):</span><span class="sxs-lookup"><span data-stu-id="80ead-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="80ead-119">"Evet"'yi ve bizim yeni bir veritabanı oluşturulur ve bizim çözüm Gezgini'ne eklenir:</span><span class="sxs-lookup"><span data-stu-id="80ead-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="80ead-120">Bizim veritabanındaki tabloları oluşturma</span><span class="sxs-lookup"><span data-stu-id="80ead-120">Creating Tables within our Database</span></span>

<span data-ttu-id="80ead-121">Şimdi yeni bir boş veritabanı sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="80ead-121">We now have a new empty database.</span></span> <span data-ttu-id="80ead-122">Bazı tablolar için ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="80ead-122">Let's add some tables to it.</span></span>

<span data-ttu-id="80ead-123">Bunu yapmak için biz "Sunucu Gezgini" sekmesi penceresi bize veritabanları ve sunucuları yönetmek sağlayan Visual Studio içinde gitmek.</span><span class="sxs-lookup"><span data-stu-id="80ead-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="80ead-124">SQL Server Express veritabanları \App depolanan\_uygulamamız veri klasörü otomatik olarak görünmesini sağlar Sunucu Gezgini içinde.</span><span class="sxs-lookup"><span data-stu-id="80ead-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="80ead-125">Biz isteğe bağlı olarak "Connect to Database" simgesini "Sunucu Gezgini" pencerenin üst kısmında (yerel ve uzak) ek SQL Server veritabanları listesine eklemek için kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="80ead-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="80ead-126">Bizim NerdDinner veritabanına – bizim azalma ve diğer kişilere RSVP kabul izlemek için depolamak için iki tablo ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="80ead-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="80ead-127">Yeni tablolar Veritabanımıza içinde "Tablo" klasöre sağ tıklayarak ve "Yeni Tablo Ekle" menü komutu seçme oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="80ead-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="80ead-128">Bu bizim tablonun şeması yapılandırma kurmamızı sağlayan bir Tablo Tasarımcısı açar.</span><span class="sxs-lookup"><span data-stu-id="80ead-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="80ead-129">Bizim "Azalma" tablosu için 10 veri sütunlarının ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="80ead-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="80ead-130">"DinnerID" sütun tablosu için benzersiz bir birincil anahtar olmasını istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="80ead-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="80ead-131">Biz "DinnerID" sütunu sağ tıklayıp "Birincil anahtarı Ayarla" menü öğesini seçerek yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="80ead-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="80ead-132">Birincil anahtar DinnerID yapmanın yanı sıra da değeri otomatik olarak artar yeni veri satırları tabloya eklenen bir "kimlik" sütunu olarak yapılandırma istiyoruz (ilk eklenen Yemeği satıra 1 DinnerID olacaktır anlamına gelir, ikinci satır eklenir bir DinnerID sahip olur 2, vb.).</span><span class="sxs-lookup"><span data-stu-id="80ead-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="80ead-133">"DinnerID" sütun seçerek bunu ve "Evet" sütununa "(kimlik)" özelliğini ayarlamak için "Sütun özellikleri" Düzenleyicisi'ni kullanın.</span><span class="sxs-lookup"><span data-stu-id="80ead-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="80ead-134">Standart kimlik Varsayılanları kullanacağız (1'den başlar ve her yeni Yemeği satırda 1 Artır):</span><span class="sxs-lookup"><span data-stu-id="80ead-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="80ead-135">Biz sonra bizim tablo kullanarak veya Ctrl-S yazarak kurtulmuş olursunuz **dosya -&gt;kaydetmek** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="80ead-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="80ead-136">Bu tablo adı için bize ister.</span><span class="sxs-lookup"><span data-stu-id="80ead-136">This will prompt us to name the table.</span></span> <span data-ttu-id="80ead-137">Biz "Azalma" adlandırın:</span><span class="sxs-lookup"><span data-stu-id="80ead-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="80ead-138">Bizim yeni azalma tablo sonra Veritabanımıza server Explorer'da içinde görünecektir.</span><span class="sxs-lookup"><span data-stu-id="80ead-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="80ead-139">Biz ardından yukarıdaki adımları yineleyin ve bir "RSVP" tablo oluşturun.</span><span class="sxs-lookup"><span data-stu-id="80ead-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="80ead-140">Bu tablo ile 3 sütuna sahip.</span><span class="sxs-lookup"><span data-stu-id="80ead-140">This table with have 3 columns.</span></span> <span data-ttu-id="80ead-141">Biz birincil anahtar olarak RsvpID sütun Kurulum ve ayrıca bir kimlik sütunu yapar:</span><span class="sxs-lookup"><span data-stu-id="80ead-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="80ead-142">Biz kaydetmek ve "Cane" adını verin.</span><span class="sxs-lookup"><span data-stu-id="80ead-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="80ead-143">Tablolar arasında yabancı anahtar ilişkisi ayarlama</span><span class="sxs-lookup"><span data-stu-id="80ead-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="80ead-144">Şimdi iki tablo Veritabanımıza içinde sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="80ead-144">We now have two tables within our database.</span></span> <span data-ttu-id="80ead-145">Böylece biz her Yemeği satır uygulayan sıfır veya daha fazla RSVP satır ilişkilendirebilir bizim son şema tasarım adımı – bu iki tablo arasında bir "bir-çok" ilişkisi kurmak için olacaktır.</span><span class="sxs-lookup"><span data-stu-id="80ead-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="80ead-146">Bu "Azalma" tablosunda "DinnerID" sütunu için yabancı anahtar ilişkisi için RSVP tablonun "DinnerID" sütunu yapılandırarak gerçekleştiririz.</span><span class="sxs-lookup"><span data-stu-id="80ead-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="80ead-147">Bunu yapmak için şu Tablo Tasarımcısı'nda RSVP tablosu sunucu Gezgini'nde çift tıklayarak açacaksınız.</span><span class="sxs-lookup"><span data-stu-id="80ead-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="80ead-148">Biz "DinnerID" sütun içerdiği, ardından seçersiniz sağ tıklatın ve "Relationshps..." i seçin</span><span class="sxs-lookup"><span data-stu-id="80ead-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="80ead-149">bağlam menüsü komutu:</span><span class="sxs-lookup"><span data-stu-id="80ead-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="80ead-150">Bu kurulum tablolar arasında ilişkiler için kullanabileceğiniz bir iletişim kutusu getirir:</span><span class="sxs-lookup"><span data-stu-id="80ead-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="80ead-151">Biz iletişim kutusuna yeni bir ilişki eklemek için "Ekle" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="80ead-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="80ead-152">İlişki eklendikten sonra biz "Tablo ve sütun belirtimi" ağaç görünümü ve özellik ızgarasının içinde iletişim kutusunun sağında düğümünü ve sağındaki "..." düğmesini tıklatıp:</span><span class="sxs-lookup"><span data-stu-id="80ead-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="80ead-153">"..." Düğmesini tıklatarak hangi tablolar ve sütunlar ilişkide bulunan, aynı zamanda ilişki adı etmemizi sağlayan belirtmeniz kurmamızı sağlayan başka bir iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="80ead-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="80ead-154">Biz "Azalma" olması için birincil anahtar tablosu değiştirin ve "DinnerID" sütun azalma tablodaki birincil anahtar olarak seçin.</span><span class="sxs-lookup"><span data-stu-id="80ead-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="80ead-155">Bizim RSVP tablosu yabancı anahtar tablosu ve RSVP olacaktır. DinnerID sütun yabancı anahtar ilişkili olacaktır:</span><span class="sxs-lookup"><span data-stu-id="80ead-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="80ead-156">Şimdi RSVP tablodaki her satır Yemeği tablosunda bir satırı ile ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="80ead-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="80ead-157">SQL Server başvuru bütünlüğü bize – korumak ve bize geçerli Yemeği satır işaret etmiyorsa yeni RSVP satır ekleme engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="80ead-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="80ead-158">Bu da bize hala kendisine başvuran satır RSVP varsa bir Yemeği satırın silinmesi engeller.</span><span class="sxs-lookup"><span data-stu-id="80ead-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="80ead-159">Veri bizim tablolarına ekleme</span><span class="sxs-lookup"><span data-stu-id="80ead-159">Adding Data to our Tables</span></span>

<span data-ttu-id="80ead-160">Şimdi bizim azalma tabloya bazı örnek veri ekleyerek tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="80ead-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="80ead-161">Sunucu Gezgini içinde sağ tıklayarak ve "Tablo verileri göster" komutunu seçerek biz verileri bir tabloya ekleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="80ead-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="80ead-162">Biz daha sonra uygulamayı uygulama başlangıç olarak kullanabileceğiniz Yemeği veri birkaç satırı ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="80ead-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="80ead-163">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="80ead-163">Next Step</span></span>

<span data-ttu-id="80ead-164">Biz Veritabanımıza oluşturma işlemini tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="80ead-164">We've finished creating our database.</span></span> <span data-ttu-id="80ead-165">Şimdi biz sorgulamak ve güncelleştirmek için kullanabileceğiniz modeli sınıfları oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="80ead-165">Let's now create model classes that we can use to query and update it.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="80ead-166">[Önceki](create-a-new-aspnet-mvc-project.md)
[sonraki](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="80ead-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
