---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: '2. Kısım: Denetleyicileri | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 2. Bölüm denetleyicileri kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878704"
---
<a name="part-2-controllers"></a><span data-ttu-id="c651b-104">Bölüm 2: denetleyicileri</span><span class="sxs-lookup"><span data-stu-id="c651b-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="c651b-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c651b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c651b-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="c651b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c651b-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c651b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c651b-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="c651b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c651b-109">2. Bölüm denetleyicileri kapsar.</span><span class="sxs-lookup"><span data-stu-id="c651b-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="c651b-110">Geleneksel web çerçeveleri ile gelen URL'ler genellikle disk üzerindeki dosyalara eşlenir.</span><span class="sxs-lookup"><span data-stu-id="c651b-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="c651b-111">Örneğin: bir URL için bir istek ister "/ Products.aspx" veya "/ Products.php" "Products.aspx" veya "Products.php" dosyası tarafından işlenen.</span><span class="sxs-lookup"><span data-stu-id="c651b-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="c651b-112">Web tabanlı MVC çerçeveleri URL'ler için sunucu kodu biraz farklı bir şekilde eşleyin.</span><span class="sxs-lookup"><span data-stu-id="c651b-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="c651b-113">Gelen URL'ler için dosya eşleme yerine bunların yerine URL'leri sınıfları yöntemlere eşlenir.</span><span class="sxs-lookup"><span data-stu-id="c651b-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="c651b-114">Bu sınıfların "Denetleyicileri" adı verilir ve kullanıcı girişini işleme gelen HTTP isteklerini işlemekten sorumlu, alma ve verilerini kaydetme ve gönderilecek yanıt belirleme geri istemciye (HTML görüntülemek, dosya indirme, farklı bir'yeniden yönlendirme URL, vb.).</span><span class="sxs-lookup"><span data-stu-id="c651b-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="c651b-115">Bir HomeController ekleme</span><span class="sxs-lookup"><span data-stu-id="c651b-115">Adding a HomeController</span></span>

<span data-ttu-id="c651b-116">Biz MVC müzik deposu uygulamamız URL'leri sitemizi giriş sayfasına işleyecek denetleyici sınıfını ekleyerek başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="c651b-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="c651b-117">Biz, ASP.NET MVC, varsayılan adlandırma kurallarına uygun ve HomeController çağırın.</span><span class="sxs-lookup"><span data-stu-id="c651b-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="c651b-118">Çözüm Gezgini içinde "Denetleyicileri" klasörü sağ tıklatın ve "Ekle" ve "Denetleyici..." seçin</span><span class="sxs-lookup"><span data-stu-id="c651b-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="c651b-119">komut:</span><span class="sxs-lookup"><span data-stu-id="c651b-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="c651b-120">Bu "Denetleyici Ekle" iletişim kutusunu getirir.</span><span class="sxs-lookup"><span data-stu-id="c651b-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="c651b-121">"HomeController" Denetleyici adı ve Ekle düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="c651b-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="c651b-122">Aşağıdaki kod ile bu HomeController.cs, yeni bir dosya oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c651b-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="c651b-123">Olabildiğince basit bir şekilde başlamak için şimdi dizin yöntemi yalnızca bir dize döndürür basit bir yöntem ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c651b-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="c651b-124">İki değişiklik vermiyoruz:</span><span class="sxs-lookup"><span data-stu-id="c651b-124">We'll make two changes:</span></span>

- <span data-ttu-id="c651b-125">ActionResult yerine bir dize döndürecek şekilde yöntemini değiştirme</span><span class="sxs-lookup"><span data-stu-id="c651b-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="c651b-126">"Hello gelen giriş" döndürmek için dönüş ifadesini değiştirin</span><span class="sxs-lookup"><span data-stu-id="c651b-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="c651b-127">Yöntem gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="c651b-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="c651b-128">Uygulamayı Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="c651b-128">Running the Application</span></span>

<span data-ttu-id="c651b-129">Şimdi Şimdi siteyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c651b-129">Now let's run the site.</span></span> <span data-ttu-id="c651b-130">Biz bizim web sunucusu başlatır ve aşağıdakilerden herhangi birini kullanarak site deneyin::</span><span class="sxs-lookup"><span data-stu-id="c651b-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="c651b-131">Hata ayıklama ⇨ hata ayıklamayı Başlat menü öğesini seçin</span><span class="sxs-lookup"><span data-stu-id="c651b-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="c651b-132">Araç çubuğundaki yeşil ok düğmesine tıklayın ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="c651b-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="c651b-133">F5 klavye kısayolunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="c651b-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="c651b-134">Yukarıdaki adımları kullanarak Projemizin derleyin ve ardından yerleşik-Visual Web başlatmak için geliştiricinin ASP.NET Geliştirme Sunucusu neden.</span><span class="sxs-lookup"><span data-stu-id="c651b-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="c651b-135">Bir bildirim ASP.NET Geliştirme Sunucusu başlatıldığını belirtmek için ekranın alt köşedeki görünür ve bağlantı noktası numarasını altında çalıştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c651b-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="c651b-136">Visual Web Developer, bizim web sunucusu URL'si gösteren bir tarayıcı penceresi otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="c651b-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="c651b-137">Bu, bize web uygulamamız hızlı şekilde denemenize yönelik izin verir:</span><span class="sxs-lookup"><span data-stu-id="c651b-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="c651b-138">Yeni bir Web sitesi oluşturduğumuz oldukça hızlı –, Tamam, üç satır işlev eklenir ve metin tarayıcıda olduğuna.</span><span class="sxs-lookup"><span data-stu-id="c651b-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="c651b-139">Bilim Doğum değil, ancak bir başlangıç değil.</span><span class="sxs-lookup"><span data-stu-id="c651b-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="c651b-140">*Not: Visual Web Developer Web sitenizi bir rastgele ücretsiz "port" numaralı çalışacak ASP.NET Geliştirme Sunucusu içerir. Yukarıdaki ekran görüntüsünde, site konumunda çalışan `http://localhost:26641/`, bağlantı noktası 26641 kullanıyor. Bağlantı noktası numarası farklı olacaktır. Biz bu öğreticide URL'SİNİN LIKE /Store/Browse hakkında konuşurken, sonra bağlantı noktası numarası geçer. Bir bağlantı noktası numarasını 26641 varsayıldığında, / deposu/için Gözat gözatma için gözatma anlamına gelir `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="c651b-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="c651b-141">Bir StoreController ekleme</span><span class="sxs-lookup"><span data-stu-id="c651b-141">Adding a StoreController</span></span>

<span data-ttu-id="c651b-142">Sitemizi giriş sayfasının uygulayan basit bir HomeController eklediğimiz.</span><span class="sxs-lookup"><span data-stu-id="c651b-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="c651b-143">Şimdi bizim müzik deposu gözatma işlevlerini uygulamak için kullanacağız başka bir denetleyici ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="c651b-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="c651b-144">Bizim depolama denetleyicisi üç senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="c651b-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="c651b-145">Bizim müzik deposundaki müzik türler listeleme sayfası</span><span class="sxs-lookup"><span data-stu-id="c651b-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="c651b-146">Tüm belirli bir tarzını müzik albümleri listeleyen bir Gözat sayfası</span><span class="sxs-lookup"><span data-stu-id="c651b-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="c651b-147">Belirli Müzik albüm hakkındaki bilgileri gösterir Ayrıntılar sayfası</span><span class="sxs-lookup"><span data-stu-id="c651b-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="c651b-148">Yeni bir StoreController sınıf ekleyerek başlayacağız..</span><span class="sxs-lookup"><span data-stu-id="c651b-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="c651b-149">Henüz yapmadıysanız, uygulama tarayıcının kapanması veya hata ayıklama ⇨ durdurma hata ayıklama menü öğesi seçilerek çalışmayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="c651b-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="c651b-150">Şimdi yeni StoreController ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c651b-150">Now add a new StoreController.</span></span> <span data-ttu-id="c651b-151">İle HomeController yaptığımız gibi Biz bu Çözüm Gezgini içinde "Denetleyicileri" klasöre sağ tıklayarak ve Add - seçme gerçekleştirirsiniz&gt;denetleyicisi menü öğesi</span><span class="sxs-lookup"><span data-stu-id="c651b-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="c651b-152">"Dizin" yöntemi, yeni StoreController zaten var.</span><span class="sxs-lookup"><span data-stu-id="c651b-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="c651b-153">Bizim müzik deposu içindeki tüm türler listeler listeleme sayfamızı uygulamak için bu "Dizin" yöntem kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="c651b-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="c651b-154">İşlenecek bizim StoreController istiyoruz iki diğer senaryolar uygulamak için iki ek yöntemleri de ekleyeceğiz: göz atma ve ayrıntıları.</span><span class="sxs-lookup"><span data-stu-id="c651b-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="c651b-155">Bu yöntemler (dizin, Gözat ve ayrıntıları) Denetleyicimizin içinde "Denetleyici eylemleri" olarak adlandırılır ve HomeController.Index () eylem yöntemiyle zaten gördüğünüz gibi kendi URL isteklerine yanıt vermek ve (genel olarak bakıldığında) hangi içerik belirlemek için iş Tarayıcı veya URL çağrılan kullanıcı için gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="c651b-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="c651b-156">Bizim StoreController uygulama theIndex() yöntemi "Merhaba gelen Store.Index()" dize döndürecek şekilde değiştirerek alacağız ve Browse() ve Details() için benzer yöntemler ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="c651b-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="c651b-157">Projeyi tekrar çalıştırın ve aşağıdaki URL'ler göz atın:</span><span class="sxs-lookup"><span data-stu-id="c651b-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="c651b-158">/ Deposu</span><span class="sxs-lookup"><span data-stu-id="c651b-158">/Store</span></span>
- <span data-ttu-id="c651b-159">/ Deposu/Gözat</span><span class="sxs-lookup"><span data-stu-id="c651b-159">/Store/Browse</span></span>
- <span data-ttu-id="c651b-160">/ Deposu/ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="c651b-160">/Store/Details</span></span>

<span data-ttu-id="c651b-161">Bu URL'lerine erişme, denetleyici eylem yöntemlerinde çağırma ve dize yanıtlar:</span><span class="sxs-lookup"><span data-stu-id="c651b-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="c651b-162">Harika olan, ancak bunlar yalnızca sabit dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="c651b-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="c651b-163">URL'den bilgi alın ve sayfa çıktısında görüntülemek için bunları dinamik olalım.</span><span class="sxs-lookup"><span data-stu-id="c651b-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="c651b-164">İlk biz URL'den bir sorgu dizesi değerini almak için Gözat eylem yöntemi değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="c651b-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="c651b-165">Biz, bizim eylem yöntemi için bir "Tarz" parametresini ekleyerek yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c651b-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="c651b-166">Biz bunu yaparken ASP.NET MVC çalıştırıldığında bizim eylem yöntemine "Tarz" adlı bir sorgu dizesi veya form post parametreleri otomatik olarak geçer.</span><span class="sxs-lookup"><span data-stu-id="c651b-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="c651b-167">*Not: Biz HttpUtility.HtmlEncode yardımcı program yöntemi kullanıcı girişini temizlenmeye için kullanmakta olduğunuz. Bizim görünüme Javascript injecting /Store/Browse gibi bir bağlantıyla birlikte gelen engel olur? Tarz =&lt;betik&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="c651b-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="c651b-168">Şimdi Şimdi Gözat/deposu/için Gözat? Tarz DISCO =</span><span class="sxs-lookup"><span data-stu-id="c651b-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="c651b-169">İleri okuma ve kimliği adlı bir giriş parametresi görüntülemek için Ayrıntılar eylemi değiştirelim</span><span class="sxs-lookup"><span data-stu-id="c651b-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="c651b-170">Önceki bizim yöntemini farklı olarak, biz kimliği değeri bir sorgu dizesi parametresi olarak katıştırma olmaz.</span><span class="sxs-lookup"><span data-stu-id="c651b-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="c651b-171">Bunun yerine biz bunu doğrudan URL içinde kendisi katıştırmak.</span><span class="sxs-lookup"><span data-stu-id="c651b-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="c651b-172">Örneğin: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="c651b-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="c651b-173">ASP.NET MVC bize kolayca herhangi bir şey yapılandırmak zorunda kalmadan bunu sağlar.</span><span class="sxs-lookup"><span data-stu-id="c651b-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="c651b-174">ASP.NET MVC'ın varsayılan yönlendirme URL kesimi sonra eylem yöntemi adı "ID" adlı bir parametre işlemek için kuraldır.</span><span class="sxs-lookup"><span data-stu-id="c651b-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="c651b-175">Eylem yöntemi kimliği adlı bir parametre varsa sonra ASP.NET MVC otomatik olarak URL kesimini için parametre olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="c651b-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="c651b-176">Uygulamayı çalıştırın ve /Store/Details/5 için göz atın:</span><span class="sxs-lookup"><span data-stu-id="c651b-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="c651b-177">Şimdi ne biz kadarki yaptıktan olduðunu:</span><span class="sxs-lookup"><span data-stu-id="c651b-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="c651b-178">Visual Web Developer kullanarak yeni bir ASP.NET MVC proje oluşturduk</span><span class="sxs-lookup"><span data-stu-id="c651b-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="c651b-179">Bir ASP.NET MVC uygulaması temel klasör yapısını ele</span><span class="sxs-lookup"><span data-stu-id="c651b-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="c651b-180">Biz bizim Web ASP.NET geliştirme sunucusu kullanarak çalıştırma öğrendiniz</span><span class="sxs-lookup"><span data-stu-id="c651b-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="c651b-181">İki denetleyici sınıf oluşturduk: bir HomeController ve bir StoreController</span><span class="sxs-lookup"><span data-stu-id="c651b-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="c651b-182">Eylem yöntemleri URL isteklerine yanıt vermek ve metin tarayıcıya dönün bizim denetleyicilerine ekledik</span><span class="sxs-lookup"><span data-stu-id="c651b-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="c651b-183">[Önceki](mvc-music-store-part-1.md)
> [sonraki](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="c651b-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
