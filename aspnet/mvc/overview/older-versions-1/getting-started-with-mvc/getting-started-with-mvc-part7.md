---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Model için doğrulama ekleme | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871954"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="7988e-104">Model için doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="7988e-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="7988e-105">tarafından [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="7988e-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="7988e-106">ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur.</span><span class="sxs-lookup"><span data-stu-id="7988e-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="7988e-107">Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7988e-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="7988e-108">Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="7988e-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="7988e-109">Bu bölümde biz uygulamamız içinde giriş doğrulamasını etkinleştirmek gerekli destek uygulamak için adımıdır.</span><span class="sxs-lookup"><span data-stu-id="7988e-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="7988e-110">Bizim veritabanı içeriği her zaman doğru olduğundan ve deneyin ve geçerli olmayan film verileri girin yararlı hata iletileri için son kullanıcılara vermek olmak.</span><span class="sxs-lookup"><span data-stu-id="7988e-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="7988e-111">Film sınıfına biraz doğrulama mantığını ekleyerek başlayın.</span><span class="sxs-lookup"><span data-stu-id="7988e-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="7988e-112">Model klasörü sağ tıklatın ve eklemek sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="7988e-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="7988e-113">Sınıfınıza film olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="7988e-113">Name your class Movie.</span></span>

<span data-ttu-id="7988e-114">Daha önce oluşturduğumuz film varlık modeli, IDE film sınıfı oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="7988e-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="7988e-115">Aslında, bir dosya ve başka bir bölümü film sınıfının parçası olabilir.</span><span class="sxs-lookup"><span data-stu-id="7988e-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="7988e-116">Bu, bir parçalı sınıf adı verilir.</span><span class="sxs-lookup"><span data-stu-id="7988e-116">This is called a Partial Class.</span></span> <span data-ttu-id="7988e-117">Başka bir dosyadan film sınıfını genişleten yapacağız.</span><span class="sxs-lookup"><span data-stu-id="7988e-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="7988e-118">Kısmi film sınıfı "arkadaş sınıfına" işaret doğrulama ipuçları sisteme verir bazı öznitelikler ile oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="7988e-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="7988e-119">Biz başlık ve gerektiği şekilde fiyat işaretleyin ve ayrıca fiyat belirli bir aralıkta olmasını ısrar.</span><span class="sxs-lookup"><span data-stu-id="7988e-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="7988e-120">Modeller klasörü sağ tıklatın ve eklemek sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="7988e-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="7988e-121">Sınıfınıza film adlandırın ve Tamam düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="7988e-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="7988e-122">İşte ne bizim kısmi film sınıfı görülüyor.</span><span class="sxs-lookup"><span data-stu-id="7988e-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="7988e-123">Uygulamanızı yeniden çalıştırın ve fiyatıyla 100'den bir filmi girmeyi deneyin.</span><span class="sxs-lookup"><span data-stu-id="7988e-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="7988e-124">Formu gönderdikten sonra bir hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="7988e-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="7988e-125">Hata sunucu tarafında yakalandı ve Form gönderilen sonra gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="7988e-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="7988e-126">ASP.NET MVC'ın yerleşik HTML Yardımcıları nasıl hata iletisini görüntüler ve değerleri bize içinde textbox öğelerin korumak akıllı dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="7988e-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="7988e-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7988e-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="7988e-128">Bu harika çalışır, ancak sunucu söz konusu önce biz kullanıcı istemci tarafında hemen söyleyebilirsiniz kaydettiyseniz iyi olur.</span><span class="sxs-lookup"><span data-stu-id="7988e-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="7988e-129">Şimdi bazı istemci tarafı doğrulama JavaScript ile etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="7988e-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="7988e-130">İstemci tarafı doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="7988e-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="7988e-131">Bizim film sınıfı bazı doğrulama öznitelikleri olduğundan, biz yalnızca birkaç JavaScript dosyaları bizim Create.aspx görünüm şablonuna ekleyin ve gerçekleşmesi istemci tarafı doğrulamasını etkinleştirmek için kod satırını ekleyin gerekir.</span><span class="sxs-lookup"><span data-stu-id="7988e-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="7988e-132">İçinden VWD bizim görünümler/film klasörüne gidin ve Create.aspx açın.</span><span class="sxs-lookup"><span data-stu-id="7988e-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="7988e-133">Çözüm Gezgini'nde betikleri klasörü açın ve aşağıdaki üç komut dosyaları için içinde sürükleyin &lt;head&gt; etiketi.</span><span class="sxs-lookup"><span data-stu-id="7988e-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="7988e-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="7988e-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="7988e-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="7988e-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="7988e-136">Bu komut dosyaları bu sırada görüntülenmesini istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="7988e-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="7988e-137">Ayrıca, Html.BeginForm yukarıda tek bu satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7988e-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="7988e-138">IDE içinde gösterilen kod aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="7988e-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="7988e-139">[![Film - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7988e-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="7988e-140">Uygulamanızı çalıştırın ve /Movies/Create tekrar ziyaret edin ve herhangi bir veri girmeden Oluştur'u tıklatın.</span><span class="sxs-lookup"><span data-stu-id="7988e-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="7988e-141">Hata iletileri hemen biz veri gönderme ile ilişkilendirmek flash sayfa olmadan sunucuya geriye görünür.</span><span class="sxs-lookup"><span data-stu-id="7988e-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="7988e-142">ASP.NET MVC, artık hem giriş (JavaScript kullanarak) istemci doğruluyor olduğundan ve sunucuda budur.</span><span class="sxs-lookup"><span data-stu-id="7988e-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="7988e-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7988e-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="7988e-144">İyi arıyor!</span><span class="sxs-lookup"><span data-stu-id="7988e-144">This is looking good!</span></span> <span data-ttu-id="7988e-145">Artık ek bir sütun veritabanına ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="7988e-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7988e-146">[Önceki](getting-started-with-mvc-part6.md)
> [sonraki](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="7988e-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
