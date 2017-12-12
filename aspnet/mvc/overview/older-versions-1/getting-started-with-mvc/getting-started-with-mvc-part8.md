---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: "Modele bir sütun ekleme | Microsoft Docs"
author: shanselman
description: "ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 4a4fc144dcbed8ad14411332df7acccc032ddf46
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="e534a-104">Modele bir sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="e534a-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="e534a-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="e534a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="e534a-106">ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="e534a-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="e534a-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="e534a-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="e534a-108">Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="e534a-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="e534a-109">Bu bölümde biz nasıl biz değişiklikler Veritabanımıza şemaya yapabilir ve değişiklikleri uygulamamız içinde işlemesi rehberlik alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="e534a-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="e534a-110">"Derecelendirme" Sütu film tabloya ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="e534a-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="e534a-111">IDE geri dönün ve veritabanı Explorer'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e534a-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="e534a-112">Film tablo sağ tıklayın ve açık tablo tanımı seçin.</span><span class="sxs-lookup"><span data-stu-id="e534a-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="e534a-113">Aşağıda görüldüğü gibi bir "Derecelendirme" sütun ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e534a-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="e534a-114">Tüm derecelendirmeleri şimdi sahip olmayan olduğundan, sütun null değerlere izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e534a-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="e534a-115">Kaydet'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e534a-115">Click Save.</span></span>

<span data-ttu-id="e534a-116">[![Film tablo düzenleme](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e534a-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="e534a-117">Ardından, çözüm Gezgini'ne dönmek ve (\Models klasöründe bulunur) Movies.edmx dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="e534a-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="e534a-118">Tasarım yüzeyine (beyaz alan) sağ tıklayın ve bir güncelleştirme modeli veritabanından seçin.</span><span class="sxs-lookup"><span data-stu-id="e534a-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="e534a-119">[![Film - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e534a-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="e534a-120">Bu "Güncelleştirme Sihirbazı'nı" başlatacak.</span><span class="sxs-lookup"><span data-stu-id="e534a-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="e534a-121">İçindeki yenileme sekmesini tıklatın ve Son'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e534a-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="e534a-122">Bizim film model sınıfı olan yeni bir sütun güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="e534a-122">Our Movie model class will then be updated with the new column.</span></span>

![Güncelleştirme Sihirbazı'nı (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="e534a-124">Son'a tıkladıktan sonra yeni bir derecelendirme sütun modelimizi film varlıkta eklemiştir görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e534a-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="e534a-125">[![Film varlık](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="e534a-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="e534a-126">Veritabanı modelinde bir sütun ekledik ancak görünümleri hakkında bilmiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="e534a-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="e534a-127">Model değişikliklerle güncelleştirme görünümleri</span><span class="sxs-lookup"><span data-stu-id="e534a-127">Update Views with Model Changes</span></span>

<span data-ttu-id="e534a-128">Yeni bir derecelendirme sütun yansıtacak şekilde görünümü şablonlarımız güncelleştirebilir birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="e534a-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="e534a-129">Görünüm Ekle iletişim kutusu oluşturarak bu görünümlere oluşturduğumuz olduğundan, bunları silin ve yeniden oluşturun biz.</span><span class="sxs-lookup"><span data-stu-id="e534a-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="e534a-130">Ancak, genellikle kişiler ilk kurulmuş nesil kendi görünüm şablonları için değişikliklerden zaten yapmış olduğunuz ve kimliği alanıyla oluşturma için yaptığımız gibi el ile yalnızca alanları silmek veya eklemek istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="e534a-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="e534a-131">\Views\Movies\Index.aspx şablonu açın ve eklemek bir &lt;th&gt;derecelendirme&lt;/th&gt; film tablo head için.</span><span class="sxs-lookup"><span data-stu-id="e534a-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="e534a-132">Benim sonra Tarz ekledim.</span><span class="sxs-lookup"><span data-stu-id="e534a-132">I added mine after Genre.</span></span> <span data-ttu-id="e534a-133">Ardından, aynı konumda yer alan sütun ancak daha düşük aşağı, bizim yeni derecelendirme çıktısını almak için bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e534a-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="e534a-134">Bizim son Index.aspx şablon şuna benzeyecektir:</span><span class="sxs-lookup"><span data-stu-id="e534a-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="e534a-135">Şimdi sonra \Views\Movies\Create.aspx şablonu açın ve yeni bizim derecelendirme özelliği için bir etiket ve metin kutusu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e534a-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="e534a-136">Bizim son Create.aspx şablon şuna ve bizim tarayıcının başlık ve ikincil değiştirelim &lt;h2&gt; "Oluşturmak bir biz burada iken film" gibi bir başlık!</span><span class="sxs-lookup"><span data-stu-id="e534a-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="e534a-137">Şimdi Oluştur sayfası eklenir veritabanında yeni bir alan var ve uygulamanızı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e534a-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="e534a-138">Yeni bir filmi - bir derecelendirme - bu kez ekleyin ve Oluştur'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e534a-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="e534a-139">[![Windows Internet Explorer - film oluştur](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="e534a-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="e534a-140">Oluştur'u sonra dizin sayfasına gönderilen ile film yeni listelenir burada yeni Derecelendirme sütununda veritabanı</span><span class="sxs-lookup"><span data-stu-id="e534a-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="e534a-141">[![Film listesi - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="e534a-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="e534a-142">Bu temel öğretici görünümlerle ilişkilendirme ve sabit kodlanmış verileri geçirmeden denetleyicileri yapmadan başlamanıza aldı.</span><span class="sxs-lookup"><span data-stu-id="e534a-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="e534a-143">Biz oluşturulur ve bir veritabanı tasarlanmış ve bazı verileri yerleştirme sonra içinde.</span><span class="sxs-lookup"><span data-stu-id="e534a-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="e534a-144">Biz veritabanından veri ve bir HTML tabloda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e534a-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="e534a-145">Ardından verileri veritabanına kendilerini Web uygulamasının içinden ekleme kullanıcı olanak tanıyan bir form oluştur eklendi.</span><span class="sxs-lookup"><span data-stu-id="e534a-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="e534a-146">Biz doğrulama eklenen sonra istemci tarafında JavaScript kullanılması doğrulama yapılan.</span><span class="sxs-lookup"><span data-stu-id="e534a-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="e534a-147">Son olarak, biz verilerin yeni bir sütun eklemek için veritabanı değiştirilmiş sonra oluşturmak ve bu yeni verileri görüntülemek için de iki sayfalarında güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="e534a-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="e534a-148">I şimdi orta düzey öğreticimizi taşımanızı öneririz "[MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" birçok videolar ve kaynakları yanı sıra [https://asp.net/mvc](https://asp.net/mvc) ASP.NET MVC hakkında daha fazla bilgi edinmek için!</span><span class="sxs-lookup"><span data-stu-id="e534a-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="e534a-149">Keyfini çıkarın!</span><span class="sxs-lookup"><span data-stu-id="e534a-149">Enjoy!</span></span>

- <span data-ttu-id="e534a-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) ve [ @shanselman ](http://twitter.com/shanselman) Twitter'da.</span><span class="sxs-lookup"><span data-stu-id="e534a-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e534a-151">Önceki</span><span class="sxs-lookup"><span data-stu-id="e534a-151">Previous</span></span>](getting-started-with-mvc-part7.md)
