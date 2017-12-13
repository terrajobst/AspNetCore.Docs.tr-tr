---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: "Ekleme bir yöntem ve görünüm oluşturun | Microsoft Docs"
author: shanselman
description: "ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 4792689087ab85be25fe186b2ec97915af448ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="33b83-104">Ekleme bir yöntem ve görünüm oluşturun</span><span class="sxs-lookup"><span data-stu-id="33b83-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="33b83-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="33b83-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="33b83-106">ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="33b83-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="33b83-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="33b83-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="33b83-108">Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="33b83-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="33b83-109">Bu bölümde biz kullanıcıların yeni filmler bizim veritabanında oluşturmak gerekli destek uygulamak için adımıdır.</span><span class="sxs-lookup"><span data-stu-id="33b83-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="33b83-110">Biz, filmler/Oluştur URL eylemi uygulayarak gerçekleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33b83-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="33b83-111">Film/Oluştur URL uygulama iki aşamalı bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="33b83-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="33b83-112">Bir kullanıcı ilk filmler/Oluştur URL'yi ziyaret ettiğinde, yeni bir filmi girmek için doldurmak bir HTML formuna göstermek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="33b83-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="33b83-113">Kullanıcı verileri sunucuya geri gönderileri ve formu gönderdiğinde, daha sonra gönderilen içeriği almak ve Veritabanımıza kaydetmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="33b83-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="33b83-114">Biz bu iki adımı iki Create() yöntemleri içinde bizim MoviesController sınıf içinde uygulama.</span><span class="sxs-lookup"><span data-stu-id="33b83-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="33b83-115">Bir yöntemi gösterir &lt;form&gt; kullanıcı yeni bir filmi oluşturmak için doldurmak olduğunu.</span><span class="sxs-lookup"><span data-stu-id="33b83-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="33b83-116">İkinci yöntem kullanıcı gönderdiğinde gönderilen veri işleme işleyecek &lt;form&gt; yedekleme sunucusuna ve bizim veritabanı içinde yeni bir filmi kaydedin.</span><span class="sxs-lookup"><span data-stu-id="33b83-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="33b83-117">Aşağıdaki kod olduğundan bu uygulanacak bizim MoviesController sınıf ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="33b83-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="33b83-118">Yukarıdaki kod tüm bizim denetleyiciyle yapmamız gereken kod içerir.</span><span class="sxs-lookup"><span data-stu-id="33b83-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="33b83-119">Şimdi bir form kullanıcıya görüntülenecek kullanacağız Create VIEW şablonu şimdi uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33b83-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="33b83-120">Biz ilk oluşturma yöntemini sağ tıklayın ve film formumuzun görünüm şablonu oluşturmak için "Görünüm Ekle" seçin.</span><span class="sxs-lookup"><span data-stu-id="33b83-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="33b83-121">Biz biz "Film" şablonu görüntüleme geçirmek için Görünüm veri sınıfı giderek ve "Şablon"Oluştur"iskele" istediğimizi belirten seçersiniz.</span><span class="sxs-lookup"><span data-stu-id="33b83-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="33b83-122">[![Görünüm Ekle](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="33b83-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="33b83-123">Ekle düğmesine tıkladıktan sonra şablonu \Movies\Create.aspx görüntüleme sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="33b83-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="33b83-124">"Oluştur" "içeriğini görüntüleme" aşağı açılır listeden seçilmediğinden Görünüm Ekle iletişim kutusu otomatik olarak "bazı varsayılan içerik bize için iskele kurulmuş".</span><span class="sxs-lookup"><span data-stu-id="33b83-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="33b83-125">Bir HTML yapı iskelesi oluşturulmuş &lt;form&gt;, doğrulama hatası için bir yer iletileri Git ve yapı iskelesi filmler hakkında bilir olduğundan, etiket ve alanları bizim sınıfın her bir özellik için oluşturulduğu.</span><span class="sxs-lookup"><span data-stu-id="33b83-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="33b83-126">Şimdi Veritabanımıza Kimliğini otomatik olarak bir filmi sağlandığından, bu alanları bu başvuru modeli kaldırın. Bizim oluşturma görünümünden kimliği.</span><span class="sxs-lookup"><span data-stu-id="33b83-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="33b83-127">Sonra 7 satırları kaldırmak &lt;gösterge&gt;alanları&lt;/legend&gt; biz istemediğiniz ID alanı gösterdikleri gibi.</span><span class="sxs-lookup"><span data-stu-id="33b83-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="33b83-128">Şimdi yeni bir filmi oluşturabilir ve veritabanına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="33b83-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="33b83-129">Biz uygulamayı yeniden çalıştırarak bunu ve ziyaret "/ filmler" "Oluştur" bağlantı URL'si ve tıklatın yeni film eklemek için.</span><span class="sxs-lookup"><span data-stu-id="33b83-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="33b83-130">[![Oluşturma - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="33b83-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="33b83-131">Biz Oluştur düğmesine tıkladığınızda, size geri (HTTP POST) yeni oluşturduğumuz /Movies/Create yöntemi için bu formu verileri nakil.</span><span class="sxs-lookup"><span data-stu-id="33b83-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="33b83-132">Yalnızca zaman sistemi otomatik olarak "numTimes" ve "name" parametre URL dışında sürdü ve daha önce bir yöntem parametrelerine eşlenen gibi sistem otomatik olarak bir GÖNDERİYE Form alanlarını alın ve nesneye eşleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33b83-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="33b83-133">Bu durumda, "ReleaseDate" ve "Title" gibi HTML alanlarındaki değerleri otomatik olarak bir filmi yeni bir örneğini doğru özelliklerini yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="33b83-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="33b83-134">At ikinci oluşturma yöntemi bizim MoviesController yeniden bakalım.</span><span class="sxs-lookup"><span data-stu-id="33b83-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="33b83-135">Bağımsız değişken olarak bir "Film" nesnesi nasıl sürdüğünü dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="33b83-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="33b83-136">Bu film nesnesi sonra bizim Create eylem yöntemi [HttpPost] sürümüne geçirildi ve veritabanına kaydedilir ve geri kaydedilen sonuç film listesinde gösterecektir İNDİS() eylem yöntemi için kullanıcı yönlendirilir:</span><span class="sxs-lookup"><span data-stu-id="33b83-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="33b83-137">[![Film listesi - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="33b83-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="33b83-138">Bizim filmler ancak doğru olduğundan ve veritabanının bize bir filmi başlığı ile kaydetmek izin vermiyor denetleniyor değil.</span><span class="sxs-lookup"><span data-stu-id="33b83-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="33b83-139">Şu hata oluştu, veritabanı önce kullanıcı söyleyebilirsiniz iyi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="33b83-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="33b83-140">Biz bu sonraki uygulamamız için doğrulama desteği ekleyerek gerçekleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="33b83-140">We'll do this next by adding validation support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="33b83-141">[Önceki](getting-started-with-mvc-part5.md)
[sonraki](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="33b83-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>
