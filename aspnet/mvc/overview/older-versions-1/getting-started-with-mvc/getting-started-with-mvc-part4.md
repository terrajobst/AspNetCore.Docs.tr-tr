---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Veritabanı oluşturma | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362362"
---
<a name="creating-a-database"></a><span data-ttu-id="bb2a4-104">Veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="bb2a4-104">Creating a Database</span></span>
====================
<span data-ttu-id="bb2a4-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="bb2a4-106">ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="bb2a4-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="bb2a4-108">Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="bb2a4-109">Bu bölümde, depolama ve alma film verilerimizi kullanacağız yeni bir SQL Express veritabanı oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="bb2a4-110">Visual Web Developer IDE içinde görünümü seçin | Sunucu Gezgini.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="bb2a4-111">Veri bağlantılarını sağ tıklayın ve Bağlantısı Ekle...</span><span class="sxs-lookup"><span data-stu-id="bb2a4-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="bb2a4-113">Veri Kaynağı Seç iletişim kutusunda, Microsoft SQL Server'ı seçin ve devam'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="bb2a4-114">Bağlantı Ekle iletişim kutusuna girin ". \SQLEXPRESS" sunucu adınız ve yeni veritabanınızın adı olarak "Filmler" girin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="bb2a4-115">[![Bağlantısı Ekle iletişim kutusu](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="bb2a4-116">Tamam'ı tıklatın ve o veritabanı oluşturmak istiyorsanız istenir.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="bb2a4-117">Evet'i seçin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-117">Select yes.</span></span>

<span data-ttu-id="bb2a4-118">[![Filmler oluşturulsun mu?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="bb2a4-119">Şimdi, boş bir veritabanı sunucu Gezgini'nde aradığınızı bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-119">Now you've got an empty database in Server Explorer.</span></span>

![Yeni Tablo Ekle](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="bb2a4-121">Tablolarda sağ tıklayın ve Tablo Ekle'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="bb2a4-122">Tablo Tasarımcısı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-122">The Table Designer will appear.</span></span> <span data-ttu-id="bb2a4-123">Kimlik, başlık, ReleaseDate, tarz ve fiyat için sütunları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="bb2a4-124">Kimlik sütunu üzerinde sağ tıklayın ve birincil anahtar tıklatın ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="bb2a4-125">Gibi görünüyor hangi benim tasarım alanları aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="bb2a4-126">[![Veritabanı Tablosu Düzenleyicisi](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="bb2a4-127">Ayrıca, kimlik sütunu seçin ve aşağıdaki sütun özellikleri altında "Kimlik belirtimi" "Evet" olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="bb2a4-128">[![IsIdentity - sütun özellikleri](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="bb2a4-129">Bitti Başardınız, araç çubuğunda Kaydet simgesine tıklayın veya dosyayı seçin | Menüden kaydedin ve tablonuzun adı "**film**" (tekil).</span><span class="sxs-lookup"><span data-stu-id="bb2a4-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="bb2a4-130">Bir veritabanı ve tablo yapılandırdığımıza göre!</span><span class="sxs-lookup"><span data-stu-id="bb2a4-130">We've got a database and a table!</span></span>

<span data-ttu-id="bb2a4-131">[![Bir ad seçin](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="bb2a4-132">Sunucu Gezgini geri dönün ve film tablo sağ tıklayın ve ardından "Show tablo verilerini."</span><span class="sxs-lookup"><span data-stu-id="bb2a4-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="bb2a4-133">Bazı veriler veritabanımızdaki sahip olması birkaç filmler girin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="bb2a4-134">[![Veritabanı tablo düzenleme](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="bb2a4-135">Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="bb2a4-135">Creating a Model</span></span>

<span data-ttu-id="bb2a4-136">Artık, Çözüm Gezgini'nde IDE'nin sağ taraftaki geri dön geçin ve modeller klasörü sağ tıklatın ve Ekle'yi seçin | Yeni öğe.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="bb2a4-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="bb2a4-138">Yeni sunduğumuz veritabanından bir varlık modeli oluşturmak için ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="bb2a4-139">Bu bizim veritabanımızdaki içinde veri sorgulama ve düzenleme için kolaylaştıran Projemizin sınıf kümesi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="bb2a4-140">İletişim kutusunun sol tarafında veri düğümünü seçin ve ardından ADO.NET varlık veri modeli öğe şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="bb2a4-141">Movies.edmx adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="bb2a4-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="bb2a4-143">"Ekle" düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-143">Click the "Add" button.</span></span> <span data-ttu-id="bb2a4-144">Bu, ardından "varlık veri modeli Sihirbazı" başlatılır.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="bb2a4-145">Açılan yeni iletişim kutusunda, veritabanından Oluştur'u seçin.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="bb2a4-146">Bir veritabanı yalnızca yaptık olduğundan, biz yalnızca Entity Framework sunduğumuz yeni bir veritabanı ve onun tablo hakkında söylemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="bb2a4-147">Yanındaki bizim sunduğumuz web uygulamanın yapılandırma veritabanı bağlantısında Kaydet'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="bb2a4-148">Şimdi, tablolar ve film kontrol onay kutusuna tıklayın ve son.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="bb2a4-149">[![Varlık veri modeli Sihirbazı](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="bb2a4-150">Şimdi biz Entity Framework Designer yeni film tablodaki bakın ve koddan erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="bb2a4-151">[![Filmler - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="bb2a4-152">Tasarım yüzeyinde, bir "Film" sınıf görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="bb2a4-153">Bu sınıf veritabanımızdaki "Film" tablosunda eşlenir ve içindeki her bir özellik bir sütun tablosu ile eşlenir.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="bb2a4-154">"Film" sınıfın her örneğini "Film" tablo içindeki satır karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="bb2a4-155">Varsayılan adlandırma ve Entity Framework tarafından kullanılan kuralları eşleme kullanmak istemiyorsanız, değiştirmek veya bunları özelleştirmek için Entity Framework designer'ı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="bb2a4-156">Bu uygulama için biz varsayılan ayarları kullanın ve yeni dosyayı farklı kaydet-olduğu.</span><span class="sxs-lookup"><span data-stu-id="bb2a4-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="bb2a4-157">Şimdi, bazı gerçek verilerle birlikte çalışalım!</span><span class="sxs-lookup"><span data-stu-id="bb2a4-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bb2a4-158">[Önceki](getting-started-with-mvc-part3.md)
> [İleri](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="bb2a4-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
