---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: "Veritabanı oluşturma | Microsoft Docs"
author: shanselman
description: "ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 22f1042555d7cdd0ca8d75f1cbde65fbb65d81cc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
<a name="creating-a-database"></a><span data-ttu-id="87869-104">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="87869-104">Creating a Database</span></span>
====================
<span data-ttu-id="87869-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="87869-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="87869-106">ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="87869-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="87869-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="87869-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="87869-108">Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="87869-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="87869-109">Bu bölümde biz depolamak ve film verilerimizi almak için kullanacağınız yeni bir SQL Express veritabanı oluşturmak için adımıdır.</span><span class="sxs-lookup"><span data-stu-id="87869-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="87869-110">Görünüm seçin ve Visual Web Developer IDE içinde | Sunucu Gezgini.</span><span class="sxs-lookup"><span data-stu-id="87869-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="87869-111">Veri bağlantılarını sağ tıklayın ve bağlantı Ekle tıklatın...</span><span class="sxs-lookup"><span data-stu-id="87869-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="87869-113">Veri Kaynağı Seç iletişim kutusunda, Microsoft SQL Server'ı seçin ve devam et seçin.</span><span class="sxs-lookup"><span data-stu-id="87869-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="87869-114">Bağlantı Ekle iletişim kutusuna girin ". \SQLEXPRESS" sunucu adınızı ve "Filmler" Yeni veritabanı adı girin.</span><span class="sxs-lookup"><span data-stu-id="87869-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="87869-115">[![Bağlantı iletişim kutusu ekleme](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="87869-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="87869-116">Tamam'ı tıklatın ve bu veritabanını oluşturmak isteyip istemediğiniz sorulur.</span><span class="sxs-lookup"><span data-stu-id="87869-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="87869-117">Evet'i seçin.</span><span class="sxs-lookup"><span data-stu-id="87869-117">Select yes.</span></span>

<span data-ttu-id="87869-118">[![Film oluşturulsun mu?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="87869-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="87869-119">Artık sunucu Gezgini'nde boş bir veritabanı açıyor.</span><span class="sxs-lookup"><span data-stu-id="87869-119">Now you've got an empty database in Server Explorer.</span></span>

![Yeni tablo ekleme](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="87869-121">Tablolarda sağ tıklatıp Tablo Ekle öğesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="87869-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="87869-122">Tablo Tasarımcısı görünür.</span><span class="sxs-lookup"><span data-stu-id="87869-122">The Table Designer will appear.</span></span> <span data-ttu-id="87869-123">Sütun kimliği, başlık, ReleaseDate, tarzını ve fiyat ekleyin.</span><span class="sxs-lookup"><span data-stu-id="87869-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="87869-124">Kimlik sütunu sağ tıklayın ve birincil anahtar tıklatın ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="87869-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="87869-125">Burada, gibi görünüyor hangi my tasarım alanlarda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="87869-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="87869-126">[![Veritabanı Tablosu Düzenleyicisi](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="87869-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="87869-127">Ayrıca, kimlik sütunu seçin ve aşağıdaki sütun özellikleri altında "Kimlik belirtimi" "Evet" olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="87869-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="87869-128">[![IsIdentity - sütun özellikleri](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="87869-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="87869-129">Bitti var olduğunda, araç çubuğunda Kaydet simgesine tıklayın veya dosyayı seçin | Menüden kaydedin ve tablonuzun adının "**film**" (tekil).</span><span class="sxs-lookup"><span data-stu-id="87869-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="87869-130">Bir veritabanı ve tablo var ki!</span><span class="sxs-lookup"><span data-stu-id="87869-130">We've got a database and a table!</span></span>

<span data-ttu-id="87869-131">[![Bir ad seçin](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="87869-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="87869-132">Sunucu Gezgini için geri dönün ve film tablo sağ tıklayın ve ardından "Tablo verileri göster."</span><span class="sxs-lookup"><span data-stu-id="87869-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="87869-133">Veritabanımıza bazı veriler nedenle birkaç filmler girin.</span><span class="sxs-lookup"><span data-stu-id="87869-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="87869-134">[![Veritabanı tablosu düzenleme](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="87869-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="87869-135">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="87869-135">Creating a Model</span></span>

<span data-ttu-id="87869-136">Şimdi, geçin geri IDE'nin sağ tarafındaki Çözüm Gezgini ve modeller klasörü sağ tıklatın ve Ekle | Yeni öğe.</span><span class="sxs-lookup"><span data-stu-id="87869-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="87869-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="87869-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="87869-138">Bizim yeni veritabanından bir varlık modeli oluşturmak için yapacağız.</span><span class="sxs-lookup"><span data-stu-id="87869-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="87869-139">Bu sınıf kümesi bize sorgulamak ve bizim veritabanındaki verileri işlemek daha kolay hale gelir Projemizin ekler.</span><span class="sxs-lookup"><span data-stu-id="87869-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="87869-140">İletişim kutusunun sol taraftaki veri düğümü seçin ve ardından ADO.NET varlık veri modeli öğesi şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="87869-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="87869-141">Movies.edmx adı.</span><span class="sxs-lookup"><span data-stu-id="87869-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="87869-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="87869-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="87869-143">"Ekle" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="87869-143">Click the "Add" button.</span></span> <span data-ttu-id="87869-144">Bu, ardından "varlık veri modeli Sihirbazı" başlatacak.</span><span class="sxs-lookup"><span data-stu-id="87869-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="87869-145">Açılır yeni iletişim kutusunda, veritabanından Oluştur'u seçin.</span><span class="sxs-lookup"><span data-stu-id="87869-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="87869-146">Bir veritabanı yalnızca yaptık olduğundan, biz yalnızca Entity Framework bizim yeni bir veritabanı ve onun tablo hakkında söylememiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="87869-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="87869-147">Yanına bizim veritabanı bağlantısı bizim web uygulamasının yapılandırma Kaydet'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="87869-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="87869-148">Şimdi, tablolar ve film denetle bitiş onay kutusunu ve'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="87869-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="87869-149">[![Varlık veri modeli Sihirbazı](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="87869-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="87869-150">Şimdi biz bizim yeni Entity Framework Designer film tabloya bakın ve kodundan erişim.</span><span class="sxs-lookup"><span data-stu-id="87869-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="87869-151">[![Film - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="87869-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="87869-152">Tasarım yüzeyine "Film" sınıfı görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87869-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="87869-153">Bu sınıf Veritabanımıza "Film" tablosunda eşler ve tablo içeren bir sütuna içindeki her bir özellik eşler.</span><span class="sxs-lookup"><span data-stu-id="87869-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="87869-154">"Film" sınıfın her örneği "Film" tablo içindeki satır karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="87869-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="87869-155">Varsayılan adlandırma ve Entity Framework tarafından kullanılan kuralları eşleme hoşlanmıyorsanız, değiştirmek veya bunları özelleştirmek için Entity Framework Tasarımcısı'nı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87869-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="87869-156">Bu uygulama için biz varsayılanları kullanın ve yalnızca dosyası olarak Kaydet-değil.</span><span class="sxs-lookup"><span data-stu-id="87869-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="87869-157">Şimdi, şimdi bazı gerçek verilerle çalışmak!</span><span class="sxs-lookup"><span data-stu-id="87869-157">Now, let's work with some real data!</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="87869-158">[Önceki](getting-started-with-mvc-part3.md)
[sonraki](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="87869-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
