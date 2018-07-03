---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Veritabanı oluşturma | Microsoft Docs
author: microsoft
description: 2. adım, NerdDinner uygulamamız için veri LCV yanıtı gönderin ve tüm Akşam Yemeği tutan veritabanı oluşturma adımları gösterilmektedir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 01c6d3c63780e492b97aa54a92f3982d4c18f9e5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371755"
---
<a name="create-a-database"></a><span data-ttu-id="87218-103">Bir veritabanı oluşturun</span><span class="sxs-lookup"><span data-stu-id="87218-103">Create a Database</span></span>
====================
<span data-ttu-id="87218-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="87218-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="87218-105">PDF'yi indirin</span><span class="sxs-lookup"><span data-stu-id="87218-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="87218-106">Adım 2 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.</span><span class="sxs-lookup"><span data-stu-id="87218-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="87218-107">2. adım, NerdDinner uygulamamız için veri LCV yanıtı gönderin ve tüm Akşam Yemeği tutan veritabanı oluşturma adımları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="87218-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="87218-108">ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.</span><span class="sxs-lookup"><span data-stu-id="87218-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="87218-109">NerdDinner 2. adım: veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="87218-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="87218-110">Bir veritabanı NerdDinner uygulamamız için tüm Akşam Yemeği ve RSVP verileri depolamak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="87218-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="87218-111">Ücretsiz SQL Server Express edition'ı kullanarak veritabanı oluşturma adımları Göster (V2 sürümleriyle kullanarak kolayca yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="87218-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="87218-112">SQL Server Express ve tam SQL Server ile tüm kodlar yazma çalışır.</span><span class="sxs-lookup"><span data-stu-id="87218-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="87218-113">Yeni bir SQL Server Express veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="87218-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="87218-114">Biz bizim web proje üzerinde sağ tıklayarak başlayın ve ardından **Add -&gt;yeni öğe** menü komutu:</span><span class="sxs-lookup"><span data-stu-id="87218-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="87218-115">Bu, Visual Studio'nun "Yeni Öğe Ekle" iletişim kutusu getirir.</span><span class="sxs-lookup"><span data-stu-id="87218-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="87218-116">Biz "Veri" kategoriye göre filtreleme ve "SQL Server veritabanı" öğe şablonu seçin:</span><span class="sxs-lookup"><span data-stu-id="87218-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="87218-117">Biz "NerdDinner.mdf" oluşturun ve Tamam isabet istiyoruz SQL Server Express veritabanı adı.</span><span class="sxs-lookup"><span data-stu-id="87218-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="87218-118">Visual Studio ardından bize Biz bu dosya için sunduğumuz \App eklemek isteyip istemediğinizi sorar\_veri dizini (zaten olan bir dizin hem okuma hem de ile Kurulum ve güvenlik ACL'leri yazma):</span><span class="sxs-lookup"><span data-stu-id="87218-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="87218-119">"Evet"'yi ve müşterilerimize yeni bir veritabanı oluşturulur ve bizim çözüm Gezgini'ne eklenir:</span><span class="sxs-lookup"><span data-stu-id="87218-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="87218-120">Bizim veritabanı içinde tablo oluşturma</span><span class="sxs-lookup"><span data-stu-id="87218-120">Creating Tables within our Database</span></span>

<span data-ttu-id="87218-121">Artık yeni bir boş veritabanı var.</span><span class="sxs-lookup"><span data-stu-id="87218-121">We now have a new empty database.</span></span> <span data-ttu-id="87218-122">Bazı tablolar için ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="87218-122">Let's add some tables to it.</span></span>

<span data-ttu-id="87218-123">Bunu yapmak için veritabanlarını ve sunucuları yönetmek bize sağlayan Visual Studio "Sunucu Gezgini" sekmesinde penceresine giderek.</span><span class="sxs-lookup"><span data-stu-id="87218-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="87218-124">SQL Server Express veritabanı \App içinde depolanan\_uygulamamız veri klasörü otomatik olarak görünür Sunucu Gezgini içinde.</span><span class="sxs-lookup"><span data-stu-id="87218-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="87218-125">İsteğe bağlı olarak "Veritabanına bağlan" simge "Sunucu Gezgini" penceresinin üst kısmındaki (yerel ve uzak) ek SQL Server veritabanlarını listesine eklemek için kullanabiliriz:</span><span class="sxs-lookup"><span data-stu-id="87218-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="87218-126">Bizim NerdDinner veritabanı – bizim azalma ve diğer RSVP edenleri bunları izlemek üzere depolamak için iki tablo ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="87218-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="87218-127">Yeni tablolar veritabanımızdaki içindeki "Tablo" klasöre sağ tıklayarak ve "Yeni Tablo Ekle" menü komutu seçme oluşturabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="87218-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="87218-128">Bu kurmamızı tablomuza şemasını yapılandırmak bir Tablo Tasarımcısı açılır.</span><span class="sxs-lookup"><span data-stu-id="87218-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="87218-129">10 veri sütunlarının "Azalma" tablomuza ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="87218-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="87218-130">"DinnerID" sütunu tablo için benzersiz bir birincil anahtar olmasını istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="87218-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="87218-131">Biz "DinnerID" sütuna sağ tıklayıp "Birincil anahtarı Ayarla" menü öğesi'i seçerek yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="87218-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="87218-132">Bir birincil anahtar DinnerID hale getirmenin yanı sıra, ayrıca değeri tabloya yeni satır veri eklendikçe otomatik olarak artırılır "kimlik" sütunu olarak yapılandırır istiyoruz (ilk eklenen Dinner satıra bir DinnerID 1 olacaktır anlamına gelir, ikinci satır eklendi bir DinnerID olacaktır 2, vb.).</span><span class="sxs-lookup"><span data-stu-id="87218-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="87218-133">Bunu "DinnerID" sütun seçerek ve ardından "(kimlik mi)" özelliği "Evet" sütununda ayarlamak için "Sütun özellikleri" Düzenleyicisi'ni kullanın.</span><span class="sxs-lookup"><span data-stu-id="87218-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="87218-134">Standart kimlik Varsayılanları kullanacağız (1'den başlar ve her yeni Dinner satırında 1 Artır):</span><span class="sxs-lookup"><span data-stu-id="87218-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="87218-135">Biz ardından tablomuza kullanarak veya Ctrl-S yazarak kazanırsınız **File -&gt;Kaydet** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="87218-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="87218-136">Bu tablo adı için bize ister.</span><span class="sxs-lookup"><span data-stu-id="87218-136">This will prompt us to name the table.</span></span> <span data-ttu-id="87218-137">Biz, bunu "Azalma" ad:</span><span class="sxs-lookup"><span data-stu-id="87218-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="87218-138">Yeni azalma tablomuza veritabanımızda yer Sunucu Gezgini içinde sonra görünür.</span><span class="sxs-lookup"><span data-stu-id="87218-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="87218-139">Biz ardından yukarıdaki adımları yineleyin ve "RSVP" tablosu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="87218-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="87218-140">Bu tablo ile 3 sütun olması.</span><span class="sxs-lookup"><span data-stu-id="87218-140">This table with have 3 columns.</span></span> <span data-ttu-id="87218-141">Biz RsvpID sütun birincil anahtar olarak ayarlama ve ayrıca bir kimlik sütunu kolaylaştırır:</span><span class="sxs-lookup"><span data-stu-id="87218-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="87218-142">Biz, kaydedin ve "RSVP" adını verin.</span><span class="sxs-lookup"><span data-stu-id="87218-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="87218-143">Tablolar arasında yabancı anahtar ilişkisi ayarlama</span><span class="sxs-lookup"><span data-stu-id="87218-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="87218-144">Artık iki tablo veritabanımızdaki içinde var.</span><span class="sxs-lookup"><span data-stu-id="87218-144">We now have two tables within our database.</span></span> <span data-ttu-id="87218-145">Böylece her Akşam Yemeği satır biz buna sıfır veya daha fazla RSVP satır ile ilişkilendirebilirsiniz: Bu iki tablo arasında bir "bir-çok" ilişki kurmak için son şema tasarım adımımız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="87218-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="87218-146">RSVP tablonun "DinnerID" sütunun "Azalma" tablosunda "DinnerID" sütunu için yabancı anahtar ilişkisi yapılandırarak bunu.</span><span class="sxs-lookup"><span data-stu-id="87218-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="87218-147">Bunu yapmak için biz Tablo Tasarımcısı içine RSVP tablosunu sunucu Gezgini'nde çift tıklayarak açacaksınız.</span><span class="sxs-lookup"><span data-stu-id="87218-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="87218-148">Ardından, "DinnerID" sütunda seçeneğini belirleyeceğiz sağ tıklayın ve "Relationshps..." bağlam menüsü komutu seçin:</span><span class="sxs-lookup"><span data-stu-id="87218-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="87218-149">Bu tablolar arasındaki ilişkileri kurulumu için kullanabileceğiniz bir iletişim kutusu getirir:</span><span class="sxs-lookup"><span data-stu-id="87218-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="87218-150">İletişim kutusuna yeni bir ilişki eklemek için "Ekle" düğmesine tıkladıktan.</span><span class="sxs-lookup"><span data-stu-id="87218-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="87218-151">İlişki eklendikten sonra biz iletişim kutusunun sağında özellik kılavuzunda "Tabloları ve sütun belirtimi" ağaç görünümü düğümü genişletin ve ardından sağındaki "..." düğmesine tıklayın:</span><span class="sxs-lookup"><span data-stu-id="87218-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="87218-152">"..." Düğmesine tıklayarak, hangi tablolar ve sütunlar ilişkide bulunan, hem de ilişki adı olanak belirtmenizi sağlayan başka bir iletişim kutusu getirir.</span><span class="sxs-lookup"><span data-stu-id="87218-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="87218-153">Biz "Azalma" olarak birincil anahtar tablosu değiştirin ve birincil anahtar olarak azalma tablo içindeki "DinnerID" sütunu seçin.</span><span class="sxs-lookup"><span data-stu-id="87218-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="87218-154">RSVP tablomuza yabancı anahtar tablosuna ve RSVP olacaktır. Yabancı anahtar DinnerID sütun ilişkilendirilecek:</span><span class="sxs-lookup"><span data-stu-id="87218-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="87218-155">RSVP tablodaki her satır Şimdi Akşam Yemeği tablosunda bir satıra ile ilişkili olacaktır.</span><span class="sxs-lookup"><span data-stu-id="87218-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="87218-156">SQL Server, bizim için – başvurusal bütünlüğü korumak ve bize geçerli bir Akşam Yemeği satıra işaret etmemesi durumunda yeni RSVP satır eklemesini engellemek.</span><span class="sxs-lookup"><span data-stu-id="87218-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="87218-157">Bu da bize varsa yine de ona başvuran satır RSVP Dinner satır silme engeller.</span><span class="sxs-lookup"><span data-stu-id="87218-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="87218-158">Bizim tablolarına veri ekleme</span><span class="sxs-lookup"><span data-stu-id="87218-158">Adding Data to our Tables</span></span>

<span data-ttu-id="87218-159">Şimdi bazı örnek veriler azalma tablomuza ekleyerek tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="87218-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="87218-160">Sunucu Gezgini içinde sağ tıklayarak ve "Tablo verilerini Göster" komutunu seçerek bir tabloya veri ekleyebiliriz:</span><span class="sxs-lookup"><span data-stu-id="87218-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="87218-161">Daha sonra uygulamayı uygulama başlangıç olarak kullanabiliriz Dinner veri birkaç satırı ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="87218-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="87218-162">Sonraki adım</span><span class="sxs-lookup"><span data-stu-id="87218-162">Next Step</span></span>

<span data-ttu-id="87218-163">Size sunduğumuz veritabanı oluşturma işlemini tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="87218-163">We've finished creating our database.</span></span> <span data-ttu-id="87218-164">Artık sorgulamak ve güncelleştirmek için kullanabiliriz model sınıfları oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="87218-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87218-165">[Önceki](create-a-new-aspnet-mvc-project.md)
> [İleri](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="87218-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
