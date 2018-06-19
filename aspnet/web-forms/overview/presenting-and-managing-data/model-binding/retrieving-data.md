---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Model bağlama ve web forms alma ve verilerle görüntüleme | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889101"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="ed870-104">Model bağlama ve web forms ile verilerini görüntüleme ve alma</span><span class="sxs-lookup"><span data-stu-id="ed870-104">Retrieving and displaying data with model binding and web forms</span></span>
====================
<span data-ttu-id="ed870-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ed870-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ed870-106">Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ed870-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ed870-107">Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ed870-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ed870-108">Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.</span><span class="sxs-lookup"><span data-stu-id="ed870-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ed870-109">Model bağlama düzeni tüm veri erişim teknolojisi ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="ed870-109">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="ed870-110">Bu öğreticide Entity Framework kullanır, ancak size en alışkın olduğu veri erişim teknolojisi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed870-110">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="ed870-111">GridView, ListView, DetailsView veya FormView denetimi gibi bir veriye bağlı sunucu denetiminden seçerek, güncelleştirme, silme ve veri oluşturma için kullanabileceğiniz yöntemler adlarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="ed870-111">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="ed870-112">Bu öğreticide, SelectMethod için bir değer belirtmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="ed870-112">In this tutorial, you will specify a value for the SelectMethod.</span></span>
> 
> <span data-ttu-id="ed870-113">Bu yöntem içinde veri almak için mantığı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ed870-113">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="ed870-114">Sonraki öğreticide UpdateMethod, DeleteMethod ve InsertMethod değerlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ed870-114">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
> 
> <span data-ttu-id="ed870-115">Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede</span><span class="sxs-lookup"><span data-stu-id="ed870-115">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ed870-116">İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="ed870-116">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ed870-117">Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="ed870-117">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="ed870-118">Öğreticide uygulamayı Visual Studio'da çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ed870-118">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="ed870-119">Ayrıca uygulama kullanılabilir Internet üzerinden bir barındırma sağlayıcısına dağıtarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed870-119">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="ed870-120">Microsoft, 10 web siteleri için ücretsiz bir web barındırma sunar bir</span><span class="sxs-lookup"><span data-stu-id="ed870-120">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="ed870-121">[Ücretsiz Azure deneme hesabı](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="ed870-121">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="ed870-122">Azure App Service Web Apps için Visual Studio web projesi dağıtma hakkında daha fazla bilgi için bkz: [Visual Studio kullanarak ASP.NET Web dağıtımı](../../deployment/visual-studio-web-deployment/introduction.md) serisi.</span><span class="sxs-lookup"><span data-stu-id="ed870-122">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="ed870-123">Bu öğretici, ayrıca, SQL Server veritabanını Azure SQL veritabanına dağıtmak için Entity Framework Code First Migrations kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ed870-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ed870-124">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="ed870-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ed870-125">Microsoft Visual Studio 2013 veya Microsoft Visual Studio Express 2013 için Web</span><span class="sxs-lookup"><span data-stu-id="ed870-125">Microsoft Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web</span></span>
>   
> 
> <span data-ttu-id="ed870-126">Bu öğreticide Visual Studio 2012 ile de çalışır, ancak kullanıcı arabirimi ve proje şablonu bazı farklılıklar olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ed870-126">This tutorial also works with Visual Studio 2012 but there will be some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="ed870-127">Yapı</span><span class="sxs-lookup"><span data-stu-id="ed870-127">What you'll build</span></span>

<span data-ttu-id="ed870-128">Bu öğreticide, gerekir:</span><span class="sxs-lookup"><span data-stu-id="ed870-128">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ed870-129">Öğrenciler kurslar içinde kayıtlı olan bir üniversite yansıtacak veri nesneleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed870-129">Build data objects that reflect a university with students enrolled in courses</span></span>
2. <span data-ttu-id="ed870-130">Derleme nesnelerinden veritabanı tabloları</span><span class="sxs-lookup"><span data-stu-id="ed870-130">Build database tables from the objects</span></span>
3. <span data-ttu-id="ed870-131">Veritabanını test verilerle doldurma</span><span class="sxs-lookup"><span data-stu-id="ed870-131">Populate the database with test data</span></span>
4. <span data-ttu-id="ed870-132">Bir web formunda veri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="ed870-132">Display data in a web form</span></span>

## <a name="set-up-project"></a><span data-ttu-id="ed870-133">Projesi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="ed870-133">Set up project</span></span>

<span data-ttu-id="ed870-134">Visual Studio 2013'te yeni bir oluşturma **ASP.NET Web uygulaması** adlı **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="ed870-134">In Visual Studio 2013, create a new **ASP.NET Web Application** called **ContosoUniversityModelBinding**.</span></span>

![Proje oluşturma](retrieving-data/_static/image2.png)

<span data-ttu-id="ed870-136">Web Forms şablonu seçin ve diğer varsayılan seçenekleri bırakın.</span><span class="sxs-lookup"><span data-stu-id="ed870-136">Select the Web Forms template, and leave the other default options.</span></span> <span data-ttu-id="ed870-137">Kurulum projesi için Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ed870-137">Click OK to setup the project.</span></span>

![Web formları seçin](retrieving-data/_static/image3.png)

<span data-ttu-id="ed870-139">İlk olarak, birkaç küçük site görünümünü özelleştirmek için değişiklik yapar.</span><span class="sxs-lookup"><span data-stu-id="ed870-139">First, you will make a couple of small changes to customize the appearance of the site.</span></span> <span data-ttu-id="ed870-140">Açık **Site.Master** dosya ve Contoso University My ASP.NET Application yerine eklenecek başlık değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ed870-140">Open the **Site.Master** file, and change the title to include Contoso University instead of My ASP.NET Application.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

<span data-ttu-id="ed870-141">Ardından, üstbilgi metni değiştirme **uygulama adı** için **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="ed870-141">Then, change the header text from **Application name** to **Contoso University**.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

<span data-ttu-id="ed870-142">Ayrıca bu siteye ilgili sayfaları yansıtacak şekilde üstbilgisinde görünen Gezinti bağlantıları Site.Master değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ed870-142">Also in Site.Master, change the navigation links that appear in the header to reflect the pages that are relevant to this site.</span></span> <span data-ttu-id="ed870-143">Ya da gerekmez **hakkında** sayfa veya **kişi** bu bağlantıları kaldırılabilmesi için sayfa.</span><span class="sxs-lookup"><span data-stu-id="ed870-143">You will not need either the **About** page or the **Contact** page so those links can be removed.</span></span> <span data-ttu-id="ed870-144">Bunun yerine, adlı bir sayfaya bir bağlantı gerekir **Öğrenciler**.</span><span class="sxs-lookup"><span data-stu-id="ed870-144">Instead, you will need a link to a page called **Students**.</span></span> <span data-ttu-id="ed870-145">Bu sayfa henüz oluşturulmadı.</span><span class="sxs-lookup"><span data-stu-id="ed870-145">This page has not been created yet.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

<span data-ttu-id="ed870-146">Kaydedin ve Site.Master kapatın.</span><span class="sxs-lookup"><span data-stu-id="ed870-146">Save and close Site.Master.</span></span>

<span data-ttu-id="ed870-147">Şimdi, Öğrenci verileri görüntülemek için web formu oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ed870-147">Now, you'll create the web form for displaying student data.</span></span> <span data-ttu-id="ed870-148">Projenize sağ tıklayın ve **Ekle** bir **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="ed870-148">Right-click your project, and **Add** a **New Item**.</span></span> <span data-ttu-id="ed870-149">Seçin **ana sayfa ile Web Form** şablonu ve adlandırın **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="ed870-149">Select the **Web Form with Master Page** template, and name it **Students.aspx**.</span></span>

![Sayfa oluşturma](retrieving-data/_static/image5.png)

<span data-ttu-id="ed870-151">Seçin **Site.Master** yeni bir web formu için ana sayfa olarak.</span><span class="sxs-lookup"><span data-stu-id="ed870-151">Select **Site.Master** as the master page for the new web form.</span></span>

## <a name="create-the-data-models-and-database"></a><span data-ttu-id="ed870-152">Veritabanı ve veri modelleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed870-152">Create the data models and database</span></span>

<span data-ttu-id="ed870-153">Nesneleri ve ilgili veritabanı tablolarını oluşturmak için Code First geçişleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="ed870-153">You will use Code First Migrations to create objects and the corresponding database tables.</span></span> <span data-ttu-id="ed870-154">Bu tablolar Öğrenciler ve bunların kurslar hakkında bilgi depolar.</span><span class="sxs-lookup"><span data-stu-id="ed870-154">These tables will store information about the students and their courses.</span></span>

<span data-ttu-id="ed870-155">Modeller klasörü adlı yeni bir sınıf ekleyin **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="ed870-155">In the Models folder, add a new class named **UniversityModels.cs**.</span></span>

![Model sınıfı oluşturma](retrieving-data/_static/image7.png)

<span data-ttu-id="ed870-157">Bu dosyada, SchoolContext, Öğrenci, kayıt ve indirmelere sınıfları aşağıdaki gibi tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="ed870-157">In this file, define the SchoolContext, Student, Enrollment, and Course classes as follows:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

<span data-ttu-id="ed870-158">Veritabanı bağlantısı ve verilerde yapılan değişiklikler yönetir DbContext SchoolContext sınıfı türetir.</span><span class="sxs-lookup"><span data-stu-id="ed870-158">The SchoolContext class derives from DbContext, which manages the database connection and changes in the data.</span></span>

<span data-ttu-id="ed870-159">Öğrenci sınıfında uygulanmış öznitelikleri fark **FirstName**, **LastName**, ve **yıl** özellikleri.</span><span class="sxs-lookup"><span data-stu-id="ed870-159">In the Student class, notice the attributes that have been applied to the **FirstName**, **LastName**, and **Year** properties.</span></span> <span data-ttu-id="ed870-160">Bu öznitelikler, bu öğreticide veri doğrulaması için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed870-160">These attributes will be used for data validation in this tutorial.</span></span> <span data-ttu-id="ed870-161">Bu sunum proje kodunu basitleştirmek için yalnızca bu özellikler veri doğrulama özniteliklerle işaretlenmiş.</span><span class="sxs-lookup"><span data-stu-id="ed870-161">To simplify the code for this demonstration project, only these properties were marked with data-validation attributes.</span></span> <span data-ttu-id="ed870-162">Gerçek bir proje ile kayıt ve indirmelere sınıfları özelliklerinde gibi doğrulanmış veriler gereken tüm özellikler için doğrulama öznitelikleri geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="ed870-162">In a real project, you would apply validation attributes to all properties that need validated data, such as properties in the Enrollment and Course classes.</span></span>

<span data-ttu-id="ed870-163">UniversityModels.cs kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ed870-163">Save UniversityModels.cs.</span></span>

<span data-ttu-id="ed870-164">Bu sınıflara göre bir veritabanını ayarlamak için Code First geçişleri için araçları kullanır.</span><span class="sxs-lookup"><span data-stu-id="ed870-164">You will use the tools for Code First Migrations to set up a database based on these classes.</span></span>

<span data-ttu-id="ed870-165">İçinde **Paket Yöneticisi Konsolu**, komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ed870-165">In **Package Manager Console**, run the command:</span></span>  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

<span data-ttu-id="ed870-166">Komut başarıyla tamamlarsa geçişler etkin bildiren bir ileti alırsınız,</span><span class="sxs-lookup"><span data-stu-id="ed870-166">If the command completes successfully you will receive a message stating migrations have been enabled,</span></span>

![geçişleri etkinleştir](retrieving-data/_static/image8.png)

<span data-ttu-id="ed870-168">Yeni bir dosya adlı bildirim **Configuration.cs** oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="ed870-168">Notice that a new file named **Configuration.cs** has been created.</span></span> <span data-ttu-id="ed870-169">Visual Studio'da oluşturulduktan sonra bu dosyayı otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="ed870-169">In Visual Studio, this file is automatically opened after it is created.</span></span> <span data-ttu-id="ed870-170">Yapılandırma sınıfı içeren bir **çekirdek** test verileri veritabanı tablolarıyla önceden doldurmak sağlayan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ed870-170">The Configuration class contains a **Seed** method which enables you to pre-populate the database tables with test data.</span></span>

<span data-ttu-id="ed870-171">Çekirdek yöntemine aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed870-171">Add the following code to the Seed method.</span></span> <span data-ttu-id="ed870-172">Eklemeniz gerekir bir **kullanarak** bildirimi **ContosoUniversityModelBinding.Models** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="ed870-172">You'll need to add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

<span data-ttu-id="ed870-173">Configuration.cs kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ed870-173">Save Configuration.cs.</span></span>

<span data-ttu-id="ed870-174">Paket Yöneticisi konsolunda komutu çalıştırmak `add-migration initial`.</span><span class="sxs-lookup"><span data-stu-id="ed870-174">In the Package Manager Console, run the command `add-migration initial`.</span></span>

<span data-ttu-id="ed870-175">Komutu çalıştırın `update-database`.</span><span class="sxs-lookup"><span data-stu-id="ed870-175">Then, run the command `update-database`.</span></span>

<span data-ttu-id="ed870-176">Bu komutu çalıştırırken özel durum almaya devam ederseniz, StudentID ve CourseID değerlerini Seed yöntemi değerlerinden farklılık mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ed870-176">If you receive an exception when running this command, it is possible that the values for StudentID and CourseID have varied from the values in the Seed method.</span></span> <span data-ttu-id="ed870-177">Veritabanında bu tablolar açın ve StudentID ve CourseID için var olan değerleri bulur.</span><span class="sxs-lookup"><span data-stu-id="ed870-177">Open those tables in the database and find existing values for StudentID and CourseID.</span></span> <span data-ttu-id="ed870-178">Bu değerleri kayıtları tablo dengeli kodunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed870-178">Add those values in the code for seeding the Enrollments table.</span></span>

<span data-ttu-id="ed870-179">Veritabanı dosyası eklendi ancak şu anda projede gizlenir.</span><span class="sxs-lookup"><span data-stu-id="ed870-179">The database file has been added, but it is currently hidden in the project.</span></span> <span data-ttu-id="ed870-180">Tıklatın **tüm dosyaları göster** dosyayı görüntüleyemiyor.</span><span class="sxs-lookup"><span data-stu-id="ed870-180">Click **Show All Files** to display the file.</span></span>

![Tüm dosyaları göster](retrieving-data/_static/image10.png)

<span data-ttu-id="ed870-182">.Mdf dosyasını şimdi uygulamada göründüğüne dikkat edin\_veri klasörü.</span><span class="sxs-lookup"><span data-stu-id="ed870-182">Notice the .mdf file now appears in the App\_Data folder.</span></span>

![Veritabanı dosyası](retrieving-data/_static/image12.png)

<span data-ttu-id="ed870-184">.Mdf dosyasını çift tıklatın ve sunucu Gezgini'ni açın.</span><span class="sxs-lookup"><span data-stu-id="ed870-184">Double-click the .mdf file and open the Server Explorer.</span></span> <span data-ttu-id="ed870-185">Tablo artık mevcut ve verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="ed870-185">The tables now exist and are populated with data.</span></span>

![veritabanı tabloları](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a><span data-ttu-id="ed870-187">Öğrenciler ve ilgili tablolardaki verileri görüntüleme</span><span class="sxs-lookup"><span data-stu-id="ed870-187">Display data from Students and related tables</span></span>

<span data-ttu-id="ed870-188">Veritabanındaki verileri ile artık bu verileri almak ve bir web sayfasında görüntülemek hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="ed870-188">With data in the database, you are now ready to retrieve that data and display it in a web page.</span></span> <span data-ttu-id="ed870-189">Kullanacağınız bir **GridView** sütunları ve satırları verileri görüntülemek için denetim.</span><span class="sxs-lookup"><span data-stu-id="ed870-189">You will use a **GridView** control to display the data in columns and rows.</span></span>

<span data-ttu-id="ed870-190">Students.aspx açarak **MainContent** yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="ed870-190">Open Students.aspx, and locate the **MainContent** placeholder.</span></span> <span data-ttu-id="ed870-191">Bu yer tutucu içinde eklemek bir **GridView** aşağıdaki kodu içeren denetim.</span><span class="sxs-lookup"><span data-stu-id="ed870-191">Within that placeholder, add a **GridView** control that includes the following code.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

<span data-ttu-id="ed870-192">Birkaç önemli kavramlar, fark bu biçimlendirme kodda vardır.</span><span class="sxs-lookup"><span data-stu-id="ed870-192">There are a couple of important concepts in this markup code for you to notice.</span></span> <span data-ttu-id="ed870-193">İlk olarak, bir değer için ayarlanmış fark **SelectMethod** GridView öğesindeki özellik.</span><span class="sxs-lookup"><span data-stu-id="ed870-193">First, notice that a value is set for the **SelectMethod** property in the GridView element.</span></span> <span data-ttu-id="ed870-194">Bu değer için GridView veri almak için kullanılan yöntemin adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="ed870-194">This value specifies the name of the method that is used for retrieving data for the GridView.</span></span> <span data-ttu-id="ed870-195">Bu yöntem sonraki adımda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ed870-195">You will create this method in the next step.</span></span> <span data-ttu-id="ed870-196">İkinci olarak, dikkat **ItemType** özelliği, daha önce oluşturduğunuz Öğrenci sınıfı ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ed870-196">Second, notice that the **ItemType** property is set to the Student class that you created earlier.</span></span> <span data-ttu-id="ed870-197">Bu değer ayarlanarak biçimlendirme kodu o sınıfın özelliklerine başvurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed870-197">By setting this value, you can refer to properties of that class in the markup code.</span></span> <span data-ttu-id="ed870-198">Örneğin, Öğrenci sınıfı kayıtları adlı bir koleksiyonu içerir.</span><span class="sxs-lookup"><span data-stu-id="ed870-198">For example, the Student class contains a collection named Enrollments.</span></span> <span data-ttu-id="ed870-199">Kullanabileceğiniz **Item.Enrollments** o koleksiyona almak ve her Öğrenci için kayıtlı kredi toplamını almak için LINQ sözdizimini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ed870-199">You can use **Item.Enrollments** to retrieve that collection and then use LINQ syntax to retrieve the sum of enrolled credits for each student.</span></span>

<span data-ttu-id="ed870-200">Arka plan kod dosyasına için belirtilen yöntemi eklemeniz gerekir **SelectMethod** değeri.</span><span class="sxs-lookup"><span data-stu-id="ed870-200">In the code-behind file, you need to add the method that is specified for the **SelectMethod** value.</span></span> <span data-ttu-id="ed870-201">Açık **Students.aspx.cs**ve ekleme **kullanarak** deyimleri için **ContosoUniversityModelBinding.Models** ve **System.Data.Entity**ad alanları.</span><span class="sxs-lookup"><span data-stu-id="ed870-201">Open **Students.aspx.cs**, and add **using** statements for the **ContosoUniversityModelBinding.Models** and **System.Data.Entity** namespaces.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

<span data-ttu-id="ed870-202">Ardından, aşağıdaki yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed870-202">Then, add the following method.</span></span> <span data-ttu-id="ed870-203">Bu yöntemin adını SelectMethod için sağlanan adıyla eşleşen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ed870-203">Notice that the name of this method matches the name you provided for SelectMethod.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

<span data-ttu-id="ed870-204">**INCLUDE** yan tümcesi bu sorgu performansını artırır ancak sorgu çalışması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ed870-204">The **Include** clause improves the performance of this query but is not essential for the query to work.</span></span> <span data-ttu-id="ed870-205">INCLUDE yan tümcesi olmadan verileri veri alındığını her veritabanı için ayrı bir sorgu ilgili gönderme içerir geç yükleme kullanılarak alınması.</span><span class="sxs-lookup"><span data-stu-id="ed870-205">Without the Include clause, the data would be retrieved using lazy loading, which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="ed870-206">INCLUDE yan tümcesini sağlayarak, ilgili verilerin tümü tek bir veritabanı sorgu alınır anlamına gelir istekli yükleme kullanarak verileri alınır.</span><span class="sxs-lookup"><span data-stu-id="ed870-206">By providing the Include clause, the data is retrieved using eager loading, which means all of the related data is retrieved through a single query of the database.</span></span> <span data-ttu-id="ed870-207">İlgili verilerin çoğu olduğu olmaması durumlarda daha fazla veri alındığından kullanılan, istekli yüklenmesi daha az verimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="ed870-207">In cases where most of the related data is not be used, eager loading can be less efficient because more data is retrieved.</span></span> <span data-ttu-id="ed870-208">İlgili verileri her kayıt için görüntülendiğinden ancak, bu durumda, istekli yüklenirken en iyi performans sağlar.</span><span class="sxs-lookup"><span data-stu-id="ed870-208">However, in this case, eager loading provides the best performance because the related data is displayed for each record.</span></span>

<span data-ttu-id="ed870-209">İlgili verileri yüklenirken performans açıklamasında hakkında daha fazla bilgi için başlıklı bölüme bakın **Lazy, Eager ve açık, ilgili veri yükleme** içinde [bir ASP Entity Framework ile ilgili verileri okuma .NET MVC uygulaması](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) konu.</span><span class="sxs-lookup"><span data-stu-id="ed870-209">For more information about performance consideration when loading related data, see the section titled **Lazy, Eager, and Explicit Loading of Related Data** in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) topic.</span></span>

<span data-ttu-id="ed870-210">Varsayılan olarak, veriler anahtar olarak işaretlenmiş özelliği değerlere göre sıralanır.</span><span class="sxs-lookup"><span data-stu-id="ed870-210">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="ed870-211">OrderBy yan tümcesinin sıralama için farklı bir değer belirtmek için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed870-211">You can add an OrderBy clause to specify a different value for sorting.</span></span> <span data-ttu-id="ed870-212">Bu örnekte, varsayılan StudentID özellik, sıralama için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed870-212">In this example, the default StudentID property is used for sorting.</span></span> <span data-ttu-id="ed870-213">İçinde [sıralama, disk belleği ve filtreleme verilerini](sorting-paging-and-filtering-data.md) konu, sıralamak için sütun seçmek kullanıcı etkinleştirecek.</span><span class="sxs-lookup"><span data-stu-id="ed870-213">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) topic, you will enable the user to select a column for sorting.</span></span>

<span data-ttu-id="ed870-214">Web uygulamanızı çalıştırın ve öğrenciler sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="ed870-214">Run your web application, and navigate to the Students page.</span></span> <span data-ttu-id="ed870-215">Öğrenciler sayfası aşağıdaki Öğrenci bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ed870-215">The Students page displays the following student information.</span></span>

![Verileri göster](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="ed870-217">Model bağlama yöntemlerinin otomatik oluşturma</span><span class="sxs-lookup"><span data-stu-id="ed870-217">Automatic generation of model binding methods</span></span>

<span data-ttu-id="ed870-218">Bu öğretici seri ile çalışırken, projenize tamamen kod yalnızca kopyalayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed870-218">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="ed870-219">Ancak, bu yaklaşımın bir dezavantajı, model bağlama yöntemleri için kodu otomatik olarak oluşturmak için Visual Studio tarafından sağlanan özellik farkında olmaktan emin olur.</span><span class="sxs-lookup"><span data-stu-id="ed870-219">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="ed870-220">Otomatik kod oluşturma kendi projelerde çalışırken, zaman ve nasıl bir işlem uygulanacağı bir fikir edinmek Yardım kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed870-220">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="ed870-221">Bu bölümde otomatik kod oluşturma özelliğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="ed870-221">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="ed870-222">Bu bölüm, yalnızca bilgi amaçlıdır ve projenizde uygulamak için gereken herhangi bir kod içermiyor.</span><span class="sxs-lookup"><span data-stu-id="ed870-222">This section is only informational and does not contain any code you need to implement in your project.</span></span>

<span data-ttu-id="ed870-223">İçin bir değer ayarlarken **SelectMethod**, **UpdateMethod**, **InsertMethod**, veya **DeleteMethod** biçimlendirme kodda özellikleri seçebileceğiniz **yeni yöntem oluşturma** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="ed870-223">When setting a value for the **SelectMethod**, **UpdateMethod**, **InsertMethod**, or **DeleteMethod** properties in the markup code, you can select the **Create New Method** option.</span></span>

![Yeni bir yöntem oluşturma](retrieving-data/_static/image18.png)

<span data-ttu-id="ed870-225">Visual Studio yalnızca arka plan kod uygun imzalı bir yöntem oluşturur, ancak ayrıca işlemi gerçekleştiren ile yardımcı olmak üzere uygulama kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ed870-225">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to assist you with performing the operation.</span></span> <span data-ttu-id="ed870-226">İlk ayarlarsanız **ItemType** otomatik kod oluşturma özelliği, oluşturulan kodu kullanmadan önce özelliği bu tür işlemler için kullanır.</span><span class="sxs-lookup"><span data-stu-id="ed870-226">If you first set the **ItemType** property before using the automatic code generation feature, the generated code will use that type for the operations.</span></span> <span data-ttu-id="ed870-227">Örneğin, UpdateMethod özelliğini ayarlarken, aşağıdaki kodu otomatik olarak oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="ed870-227">For example, when setting the UpdateMethod property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="ed870-228">Yeniden projenize eklemek Yukarıdaki kod gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ed870-228">Again, the above code does not need to be added to your project.</span></span> <span data-ttu-id="ed870-229">Sonraki öğreticide, güncelleştirme, silme ve yeni veri eklemek için yöntemleri gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="ed870-229">In the next tutorial, you will implement methods for updating, deleting and adding new data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ed870-230">Sonuç</span><span class="sxs-lookup"><span data-stu-id="ed870-230">Conclusion</span></span>

<span data-ttu-id="ed870-231">Bu öğreticide, veri modeli sınıflarını oluşturulur ve bu sınıflardan bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ed870-231">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="ed870-232">Veritabanı tablolarını test verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="ed870-232">You filled the database tables with test data.</span></span> <span data-ttu-id="ed870-233">Model bağlama veritabanından veri almak için kullanılır ve ardından verileri bir GridView görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ed870-233">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="ed870-234">Sonraki [öğretici](updating-deleting-and-creating-data.md) bu dizide, güncelleştirme, silme ve veri oluşturma olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="ed870-234">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you will enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ed870-235">Next</span><span class="sxs-lookup"><span data-stu-id="ed870-235">Next</span></span>](updating-deleting-and-creating-data.md)
