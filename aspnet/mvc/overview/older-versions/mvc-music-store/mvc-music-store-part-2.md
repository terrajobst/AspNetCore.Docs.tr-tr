---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: '2. Bölüm: Denetleyicileri | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 2 denetleyicileri kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: a854feff675cae302b9927d209808257b7d087cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381809"
---
<a name="part-2-controllers"></a><span data-ttu-id="515a9-104">2. Bölüm: denetleyicileri</span><span class="sxs-lookup"><span data-stu-id="515a9-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="515a9-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="515a9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="515a9-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="515a9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="515a9-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="515a9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="515a9-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="515a9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="515a9-109">Bölüm 2 denetleyicileri kapsar.</span><span class="sxs-lookup"><span data-stu-id="515a9-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="515a9-110">Geleneksel web çerçeveleri ile gelen URL'ler diskteki dosyaları genellikle eşlenir.</span><span class="sxs-lookup"><span data-stu-id="515a9-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="515a9-111">Örneğin: bir URL isteği ister "/ Products.aspx" veya "/ Products.php" bir "Products.aspx" veya "Products.php" dosyası tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="515a9-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="515a9-112">Web tabanlı MVC çerçeveleri URL'leri için sunucu kodu biraz farklı bir şekilde eşleştirin.</span><span class="sxs-lookup"><span data-stu-id="515a9-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="515a9-113">Gelen URL'ler için dosya eşleme yerine bunların yerine URL'leri sınıfları yöntemlerde eşleyin.</span><span class="sxs-lookup"><span data-stu-id="515a9-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="515a9-114">Bu sınıfların "Denetleyicileri" olarak adlandırılır ve kullanıcı girişini işleme gelen HTTP isteklerini işlemekten sorumlu oldukları, alma ve verilerini kaydetme ve gönderilecek yanıt belirleyen yedekleme istemciye (HTML görüntülemek, dosya indirme, farklı bir'yeniden yönlendirme URL, vb.).</span><span class="sxs-lookup"><span data-stu-id="515a9-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="515a9-115">Bir HomeController ekleme</span><span class="sxs-lookup"><span data-stu-id="515a9-115">Adding a HomeController</span></span>

<span data-ttu-id="515a9-116">Biz, MVC müzik Store uygulamamız URL'leri sitemizi giriş sayfasına işleyecek bir denetleyici sınıfı ekleyerek başlarsınız.</span><span class="sxs-lookup"><span data-stu-id="515a9-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="515a9-117">Biz, ASP.NET MVC varsayılan adlandırma kurallarına uygun ve HomeController çağırın.</span><span class="sxs-lookup"><span data-stu-id="515a9-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="515a9-118">Çözüm Gezgini içinde "Denetleyicileri" klasörü sağ tıklatın ve "Ekle" ve "Denetleyici..." komutunu seçin:</span><span class="sxs-lookup"><span data-stu-id="515a9-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="515a9-119">Bu, "Denetleyici Ekle" iletişim kutusu getirir.</span><span class="sxs-lookup"><span data-stu-id="515a9-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="515a9-120">Denetleyici "HomeController" olarak adlandırın ve Ekle düğmesine basın.</span><span class="sxs-lookup"><span data-stu-id="515a9-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="515a9-121">Bu, aşağıdaki kod ile HomeController.cs, yeni bir dosya oluşturur:</span><span class="sxs-lookup"><span data-stu-id="515a9-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="515a9-122">Olabildiğince basit bir şekilde başlamak için şimdi dizin yöntemi yalnızca bir dize döndüren basit bir yöntem ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="515a9-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="515a9-123">İki değişiklik oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="515a9-123">We'll make two changes:</span></span>

- <span data-ttu-id="515a9-124">ActionResult yerine bir dize döndürecek şekilde yöntemini değiştirme</span><span class="sxs-lookup"><span data-stu-id="515a9-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="515a9-125">"Hello gelen giriş" döndürülecek dönüş deyimi değiştirin</span><span class="sxs-lookup"><span data-stu-id="515a9-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="515a9-126">Yöntemi gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="515a9-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="515a9-127">Uygulamayı Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="515a9-127">Running the Application</span></span>

<span data-ttu-id="515a9-128">Artık site çalıştıralım.</span><span class="sxs-lookup"><span data-stu-id="515a9-128">Now let's run the site.</span></span> <span data-ttu-id="515a9-129">Biz de bizim web sunucusunu başlatmak ve aşağıdakilerden birini kullanarak site deneyin:</span><span class="sxs-lookup"><span data-stu-id="515a9-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="515a9-130">Hata ayıklama ⇨ hata ayıklamayı Başlat menü öğesini seçin</span><span class="sxs-lookup"><span data-stu-id="515a9-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="515a9-131">Araç çubuğundaki yeşil ok düğmesine tıklayın ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="515a9-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="515a9-132">F5 klavye kısayolunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="515a9-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="515a9-133">Yukarıdaki adımları kullanarak projemizdeki derleyin ve ardından yerleşik-görsel Web başlatmak için geliştiricinin ASP.NET Geliştirme Sunucusu neden.</span><span class="sxs-lookup"><span data-stu-id="515a9-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="515a9-134">Bir bildirim ASP.NET Geliştirme Sunucusu başlatıldıktan belirtmek için ekranın alt köşesinde görünür ve bağlantı noktası numarasını altında çalışır olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="515a9-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="515a9-135">Visual Web Developer, bizim web sunucusu URL'si gösteren bir tarayıcı penceresi otomatik olarak açılır.</span><span class="sxs-lookup"><span data-stu-id="515a9-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="515a9-136">Bu web uygulamamıza'ı hızlıca denemeniz bize izin verir:</span><span class="sxs-lookup"><span data-stu-id="515a9-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="515a9-137">Yeni bir Web sitesi oluşturduğumuz oldukça hızlı –, Tamam, bir üç satır içi işlev eklendi ve metin bir tarayıcıda yapılandırdığımıza göre.</span><span class="sxs-lookup"><span data-stu-id="515a9-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="515a9-138">Bilimi Doğum değil, ancak bir başlangıçtır.</span><span class="sxs-lookup"><span data-stu-id="515a9-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="515a9-139">*Not: Visual Web Developer sitenizi rastgele ücretsiz "bağlantı" noktalarında çalışır ASP.NET Geliştirme Sunucusu içerir. Yukarıdaki ekran görüntüsünde, sitesinin çalışır durumda `http://localhost:26641/`, bağlantı noktası 26641 kullanıyor. Bağlantı noktası numaranızı farklı olacaktır. Biz bu öğreticide URL'leri gibi /Store/Browse hakkında konuşurken, bağlantı noktası numarasından sonra geçer. Bir bağlantı noktası numarasını 26641 varsayıldığında, tarama/Store/dizinine göz atma göz anlamına gelir `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="515a9-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="515a9-140">Bir StoreController ekleme</span><span class="sxs-lookup"><span data-stu-id="515a9-140">Adding a StoreController</span></span>

<span data-ttu-id="515a9-141">Sitemizi ana sayfası uygulayan bir basit HomeController ekledik.</span><span class="sxs-lookup"><span data-stu-id="515a9-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="515a9-142">Artık bizim müzik deposu gözatma işlevselliğini uygulamak için kullanacağız başka bir denetleyici ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="515a9-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="515a9-143">Deposu denetleyicimizin üç senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="515a9-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="515a9-144">Bizim müzik deposu içinde müzik türleri listesi sayfası</span><span class="sxs-lookup"><span data-stu-id="515a9-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="515a9-145">Tüm müzik albümleri, belirli bir türe listeleyen bir göz atma sayfasından</span><span class="sxs-lookup"><span data-stu-id="515a9-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="515a9-146">Belirli bir müzik Albüm hakkında bilgi gösteren Ayrıntılar sayfası</span><span class="sxs-lookup"><span data-stu-id="515a9-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="515a9-147">Yeni bir StoreController sınıf ekleyerek başlayacağız..</span><span class="sxs-lookup"><span data-stu-id="515a9-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="515a9-148">Henüz yapmadıysanız, uygulama tarayıcının kapanması veya hata ayıklama ⇨ hata ayıklamayı Durdur menü öğesi seçilerek çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="515a9-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="515a9-149">Şimdi yeni StoreController ekleyin.</span><span class="sxs-lookup"><span data-stu-id="515a9-149">Now add a new StoreController.</span></span> <span data-ttu-id="515a9-150">İle HomeController yaptığımız gibi Çözüm Gezgini içinde "Denetleyicileri" klasöre sağ tıklayarak ve Add - seçerek bunu&gt;denetleyicisi menü öğesi</span><span class="sxs-lookup"><span data-stu-id="515a9-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="515a9-151">Yeni sunduğumuz StoreController, "Index" yöntemi zaten var.</span><span class="sxs-lookup"><span data-stu-id="515a9-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="515a9-152">Bizim müzik deposu içindeki tüm türleri listeler listeleme sayfamızı uygulamak için bu "Index" yöntem kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="515a9-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="515a9-153">İşlemek için sunduğumuz StoreController istiyoruz iki diğer senaryolar uygulamak için iki ek yöntem de ekleyeceğiz: göz atma ve ayrıntıları.</span><span class="sxs-lookup"><span data-stu-id="515a9-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="515a9-154">Bu yöntemler (dizin, göz atma ve ayrıntıları) Denetleyicimizin içinde "Denetleyici eylemlerini" olarak adlandırılır ve HomeController.Index () eylem yöntemine önceden gördüğünüz gibi kendi URL taleplerine yanıt vereceğini ve içeriği (genel olarak bakıldığında) belirlemek için iş Tarayıcı veya URL çağrılan kullanıcı geri gönderilmesi.</span><span class="sxs-lookup"><span data-stu-id="515a9-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="515a9-155">StoreController kararlılığımızın theIndex() yöntemi "Hello gelen Store.Index()" dize döndürecek şekilde değiştirerek başlayacağız ve Browse() ve Details() için benzer yöntemler ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="515a9-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="515a9-156">Projeyi yeniden çalıştırın ve aşağıdaki URL'ler göz atın:</span><span class="sxs-lookup"><span data-stu-id="515a9-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="515a9-157">/ Store</span><span class="sxs-lookup"><span data-stu-id="515a9-157">/Store</span></span>
- <span data-ttu-id="515a9-158">/ Store/Gözat</span><span class="sxs-lookup"><span data-stu-id="515a9-158">/Store/Browse</span></span>
- <span data-ttu-id="515a9-159">/ Store/ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="515a9-159">/Store/Details</span></span>

<span data-ttu-id="515a9-160">Bu URL'lerine erişme denetleyici eylem yöntemlerinde çağırmak ve dize yanıtlarını döndürmek:</span><span class="sxs-lookup"><span data-stu-id="515a9-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="515a9-161">Bu harika bir deneyimdir ancak bunlar yalnızca sabit dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="515a9-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="515a9-162">URL'den bilgi alın ve içinde sayfa çıktısını görüntülemek için bunları dinamik olalım.</span><span class="sxs-lookup"><span data-stu-id="515a9-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="515a9-163">URL bir sorgu dizesi değerini almak için göz atma eylem yönteminin ilk değiştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="515a9-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="515a9-164">"Tarzı" parametresi bizim eylem yöntemine ekleyerek bunu.</span><span class="sxs-lookup"><span data-stu-id="515a9-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="515a9-165">Bunu, ASP.NET MVC çağrıldığında bizim eylem yöntemine "tarzı" adlı bir sorgu dizesi veya form gönderi parametresi otomatik olarak geçer.</span><span class="sxs-lookup"><span data-stu-id="515a9-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="515a9-166">*Not: Kullanıcı girişi temizleyin için HttpUtility.HtmlEncode yardımcı yöntem kullanıyoruz. Bizim görünüme Javascript ekleme /Store/Browse gibi bir bağlantı içeren gelen engel olur? Tarz =&lt;betik&gt;window.location='http://hackersite.com'&lt;/SCRIPT&gt;.*</span><span class="sxs-lookup"><span data-stu-id="515a9-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="515a9-167">Şimdi Şimdi Gözat/Store/dizinine göz atma? Tarz DISCO =</span><span class="sxs-lookup"><span data-stu-id="515a9-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="515a9-168">Sonraki okuma ve kimliğine adlı giriş parametresi görüntülemek için Ayrıntılar eylemi değiştirelim</span><span class="sxs-lookup"><span data-stu-id="515a9-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="515a9-169">Bizim önceki yöntem, biz kimlik değerini bir sorgu dizesi parametresi olarak ekleme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="515a9-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="515a9-170">Bunun yerine biz bunu doğrudan URL içinde kendisi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="515a9-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="515a9-171">Örneğin: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="515a9-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="515a9-172">ASP.NET MVC herhangi bir şey yapılandırmak zorunda kalmadan kolayca bunun olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="515a9-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="515a9-173">ASP.NET MVC'nin varsayılan yönlendirme bir URL kesimi eylem yöntem adından sonra "ID" adlı bir parametre olarak değerlendirilecek kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="515a9-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="515a9-174">Eylem yönteminizi kimliği adlı bir parametreye sahipse sonra ASP.NET MVC otomatik olarak URL kesimi, parametre olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="515a9-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="515a9-175">Uygulamayı çalıştırmak ve /Store/Details/5 için göz atın:</span><span class="sxs-lookup"><span data-stu-id="515a9-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="515a9-176">Şimdi şu ana kadar yaptıklarımızı özeti:</span><span class="sxs-lookup"><span data-stu-id="515a9-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="515a9-177">Visual Web Developer ile yeni bir ASP.NET MVC projesi oluşturduk</span><span class="sxs-lookup"><span data-stu-id="515a9-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="515a9-178">Bir ASP.NET MVC uygulaması temel bir klasör yapısını Bahsettiğimiz</span><span class="sxs-lookup"><span data-stu-id="515a9-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="515a9-179">ASP.NET geliştirme sunucusu kullanarak sitemizin çalıştırma öğrendik.</span><span class="sxs-lookup"><span data-stu-id="515a9-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="515a9-180">İki denetleyici sınıflarına oluşturduk: bir HomeController ve bir StoreController</span><span class="sxs-lookup"><span data-stu-id="515a9-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="515a9-181">Eylem yöntemleri için URL taleplerine yanıt vereceğini ve tarayıcıya dönüş metni bizim denetleyicileri ekledik</span><span class="sxs-lookup"><span data-stu-id="515a9-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="515a9-182">[Önceki](mvc-music-store-part-1.md)
> [İleri](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="515a9-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
