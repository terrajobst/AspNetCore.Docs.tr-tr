---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Modele doğrulama ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 1a8c186d5a6b00aaf1061bb4025f4f062203a7df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402987"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="fee46-104">Modele doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="fee46-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="fee46-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="fee46-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="fee46-106">ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="fee46-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="fee46-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="fee46-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="fee46-108">Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.</span><span class="sxs-lookup"><span data-stu-id="fee46-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="fee46-109">Bu bölümde giriş doğrulaması uygulamamız içinde etkinleştirmek için gereken destek uygulamak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="fee46-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="fee46-110">Veritabanı içeriklerimizde her zaman doğru olduğundan ve deneyin ve geçersiz film verileri girin son kullanıcılara yardımcı hata iletileri sağlar sağlamak.</span><span class="sxs-lookup"><span data-stu-id="fee46-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="fee46-111">Biz, film sınıfına biraz Doğrulama mantığı ekleyerek başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="fee46-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="fee46-112">Model klasörü sağ tıklatın ve eklemek sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="fee46-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="fee46-113">Sınıfınıza film adı.</span><span class="sxs-lookup"><span data-stu-id="fee46-113">Name your class Movie.</span></span>

<span data-ttu-id="fee46-114">Daha önce oluşturduğumuz film varlık modeli, IDE bir film sınıf oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="fee46-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="fee46-115">Aslında, film sınıfın parçası, bir dosya ve başka bir bölümü olabilir.</span><span class="sxs-lookup"><span data-stu-id="fee46-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="fee46-116">Bu, kısmi bir sınıf adı verilir.</span><span class="sxs-lookup"><span data-stu-id="fee46-116">This is called a Partial Class.</span></span> <span data-ttu-id="fee46-117">Başka bir dosyadan film sınıfını genişleten oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="fee46-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="fee46-118">Kısmi film sınıfı, işaret ettiği "dost class" doğrulama ipuçları sisteme verir. bazı öznitelikler ile oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="fee46-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="fee46-119">Biz başlık ve fiyat gerekli olarak işaretlemek ve fiyat belirli bir aralıkta olması da ısrar.</span><span class="sxs-lookup"><span data-stu-id="fee46-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="fee46-120">Modeller klasörü sağ tıklatın ve eklemek sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="fee46-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="fee46-121">Sınıfınıza film adı ve Tamam düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fee46-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="fee46-122">İşte ne gibi bizim kısmi film sınıfı görünür.</span><span class="sxs-lookup"><span data-stu-id="fee46-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="fee46-123">Uygulamanızı yeniden çalıştırın ve bir filmi fiyatı 100'den girmeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="fee46-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="fee46-124">Formu gönderdikten sonra bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="fee46-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="fee46-125">Hata, sunucu tarafında yakalandı ve Form gönderildikten sonra gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="fee46-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="fee46-126">ASP.NET MVC'ın yerleşik HTML Yardımcıları nasıl hata iletisi görüntüler ve değerleri bizim için metin kutusu öğeleri içinde korumak akıllı dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="fee46-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="fee46-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fee46-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="fee46-128">Bu mükemmel çalışıyor, ancak sunucu söz konusu önce biz kullanıcı istemci tarafında hemen söyleyebilirsiniz iyi olurdu.</span><span class="sxs-lookup"><span data-stu-id="fee46-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="fee46-129">Şimdi bazı istemci tarafı doğrulama JavaScript etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="fee46-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="fee46-130">İstemci tarafı doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="fee46-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="fee46-131">Bizim film sınıfı bazı doğrulama öznitelikleri olduğundan, biz yalnızca birkaç JavaScript dosyaları bizim Create.aspx görünümü şablonuna ekleyin ve bir gerçekleşmesi istemci tarafı doğrulamasını etkinleştirmek için kod satırını ekleyin gerekir.</span><span class="sxs-lookup"><span data-stu-id="fee46-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="fee46-132">İçinden VWD bizim görünümler/film klasörüne gidin ve Create.aspx açın.</span><span class="sxs-lookup"><span data-stu-id="fee46-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="fee46-133">Çözüm Gezgini'nde betikleri klasörü açın ve aşağıdaki üç betikleri içinde sürükleyin &lt;baş&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="fee46-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="fee46-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="fee46-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="fee46-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="fee46-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="fee46-136">Bu komut dosyaları, bu sırada görünmesini istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="fee46-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="fee46-137">Ayrıca, yukarıda Html.BeginForm tek bu satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fee46-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="fee46-138">IDE içinde gösterilen kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fee46-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="fee46-139">[![Filmler - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fee46-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="fee46-140">Uygulamanızı çalıştırın ve /Movies/Create yeniden ziyaret edebilir ve herhangi bir veri girmeye gerek kalmadan Oluştur'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fee46-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="fee46-141">Hata iletileri hemen sayfanın biz veri gönderen ile ilişkilendirmek flash olmadan tüm sürümlerde sunucuya görünür.</span><span class="sxs-lookup"><span data-stu-id="fee46-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="fee46-142">ASP.NET MVC, artık hem de giriş (JavaScript kullanan) istemci doğruluyor olduğundan ve sunucuda budur.</span><span class="sxs-lookup"><span data-stu-id="fee46-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="fee46-143">[![Oluştur - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fee46-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="fee46-144">Bu iyi görünüyor!</span><span class="sxs-lookup"><span data-stu-id="fee46-144">This is looking good!</span></span> <span data-ttu-id="fee46-145">Artık ek bir sütun veritabanına ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="fee46-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fee46-146">[Önceki](getting-started-with-mvc-part6.md)
> [İleri](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="fee46-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
