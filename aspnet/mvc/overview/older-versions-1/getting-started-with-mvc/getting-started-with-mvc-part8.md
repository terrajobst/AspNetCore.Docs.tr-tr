---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Modele sütun ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 22a6c4e5a07e81d5876cc442e68926094e3a243d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751918"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="ed7ff-104">Modele sütun ekleme</span><span class="sxs-lookup"><span data-stu-id="ed7ff-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="ed7ff-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="ed7ff-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="ed7ff-106">ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="ed7ff-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="ed7ff-108">Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="ed7ff-109">Bu bölümde rehberlik nasıl biz şemasını veritabanımızdaki için değişiklik ve değişiklikleri uygulamamız içinde işlemek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="ed7ff-110">Bir "Değerlendirme" Sütu film tabloya ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="ed7ff-111">IDE'ye dönün ve veritabanı Explorer'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="ed7ff-112">Film tablo sağ tıklayın ve açık tablo tanımını seçin.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="ed7ff-113">Aşağıda görüldüğü gibi bir "Sıralama" sütun ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="ed7ff-114">Biz tüm derecelendirmeleri artık yoksa sütunu null değerlere izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="ed7ff-115">Kaydet'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-115">Click Save.</span></span>

<span data-ttu-id="ed7ff-116">[![Filmler tablo düzenleme](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ed7ff-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="ed7ff-117">Ardından, çözüm Gezgini'ne dönün ve (hangi \Models klasöründe bulunur) Movies.edmx dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="ed7ff-118">Tasarım yüzeyinde (beyaz alanı) sağ tıklayın ve veritabanından bir güncelleştirme modeli seçin.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="ed7ff-119">[![Filmler - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ed7ff-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="ed7ff-120">Bu, "Güncelleştirme Sihirbazı" başlatılır.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="ed7ff-121">İçindeki yenileme sekmesine tıklayın ve Son'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="ed7ff-122">Bizim film model sınıfı ile yeni bir sütun güncelleştirilecektir.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-122">Our Movie model class will then be updated with the new column.</span></span>

![Güncelleştirme Sihirbazı'nı (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="ed7ff-124">Son'a tıkladıktan sonra yeni bir derecelendirme sütun modelimizi film varlıkta eklenmiştir görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="ed7ff-125">[![Film varlık](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="ed7ff-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="ed7ff-126">Veritabanı modeli bir sütun ekledik ancak bu konuda görünümleri bilmiyorum.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="ed7ff-127">Görünüm modeli değişikliklerle güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="ed7ff-127">Update Views with Model Changes</span></span>

<span data-ttu-id="ed7ff-128">Yeni bir derecelendirme sütun yansıtacak şekilde görünümü şablonlarımızı güncelleştiriyoruz birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="ed7ff-129">Görünüm Ekle iletişim kutusu oluşturarak bu görünümler oluşturduğumuz olduğundan, biz bunları silin ve yeniden oluşturmanız.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="ed7ff-130">Ancak, genellikle kişiler ilk iskele kurulmuş oluşturma için şablonları göster değişikliklerden zaten yapmış olduğunuz ve ekleme veya oluşturma kimliği alanı ile yaptığımız gibi el ile alanları silmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="ed7ff-131">\Views\Movies\Index.aspx şablonunu açın ve eklemek bir &lt;th&gt;derecelendirme&lt;/th&gt; film tablonun karşılaştırması.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="ed7ff-132">Benim sonra Tarz ekledim.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-132">I added mine after Genre.</span></span> <span data-ttu-id="ed7ff-133">Ardından, aynı sütun konumu ancak Alt tuşunu, sunduğumuz yeni derecelendirme çıktısını almak için bir satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="ed7ff-134">Bizim son Index.aspx şablon şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="ed7ff-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="ed7ff-135">Şimdi ardından \Views\Movies\Create.aspx şablonunu açın ve yeni bizim derecelendirme özelliği için bir etiket ve metin kutusu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ed7ff-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="ed7ff-136">Bizim son Create.aspx şablon şuna ve bizim tarayıcının başlık ve ikincil değiştirelim &lt;h2&gt; başlığı "Oluşturma bir biz burada iken film" gibi bir şey!</span><span class="sxs-lookup"><span data-stu-id="ed7ff-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="ed7ff-137">Uygulamanızı çalıştırın ve artık, yeni bir alan Oluştur sayfasına eklenir veritabanında aradığınızı bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="ed7ff-138">Bu sefer bir derecelendirme - yeni bir film - ekleyin ve Oluştur'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="ed7ff-139">[![Windows Internet Explorer - film oluşturma](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="ed7ff-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="ed7ff-140">Oluştur'a tıklayın, sonra dizin sayfasına gönderildiniz ile yeni film listelenir burada veritabanındaki yeni Derecelendirme sütunu</span><span class="sxs-lookup"><span data-stu-id="ed7ff-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="ed7ff-141">[![Film listesi - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="ed7ff-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="ed7ff-142">Bu temel bir öğretici videodan bunları görünümleri ile ilişkilendirme ve geçici olarak kodlanmış veri geçirme denetleyicileri yapmadan başlamanıza aldı.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="ed7ff-143">Biz oluşturulan ve bir veritabanı tasarlanmış ve bazı verilerinizden içinde.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="ed7ff-144">Biz, verileri veritabanından alınan ve HTML tablosu halinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="ed7ff-145">Ardından verileri veritabanına kendilerini Web uygulamasının içinden ekleyin kullanıcının bir form oluştur ekledik.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="ed7ff-146">Biz doğrulama eklenir ve ardından JavaScript kullanan istemci tarafında doğrulama yapılan.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="ed7ff-147">Son olarak, biz veritabanı veri yeni bir sütun içerecek şekilde değiştirilmiş sonra iki sayfalarımızın oluşturmak ve bu yeni verileri görüntülemek için güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="ed7ff-148">Orta düzey öğreticimize geçmek için artık öneriyoruz "[MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" yanı sıra birçok videoları ve kaynaklarınıza [ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC hakkında daha fazla bilgi edinmek için!</span><span class="sxs-lookup"><span data-stu-id="ed7ff-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="ed7ff-149">Keyfini çıkarın!</span><span class="sxs-lookup"><span data-stu-id="ed7ff-149">Enjoy!</span></span>

- <span data-ttu-id="ed7ff-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) ve [ @shanselman ](http://twitter.com/shanselman) Twitter'da.</span><span class="sxs-lookup"><span data-stu-id="ed7ff-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ed7ff-151">Önceki</span><span class="sxs-lookup"><span data-stu-id="ed7ff-151">Previous</span></span>](getting-started-with-mvc-part7.md)
