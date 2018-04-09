---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: '3. Kısım: Görünümleri ve ViewModels | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 3. kısım, görünümler ve ViewModels kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="d0e7a-104">3. Kısım: Görünümleri ve ViewModels</span><span class="sxs-lookup"><span data-stu-id="d0e7a-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="d0e7a-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="d0e7a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="d0e7a-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="d0e7a-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="d0e7a-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="d0e7a-109">3. kısım, görünümler ve ViewModels kapsar.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="d0e7a-110">Şu ana kadar biz yalnızca dizeleri denetleyicisi eylemlerden döndürme.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="d0e7a-111">Denetleyicileri nasıl çalıştığı hakkında bir fikir edinmek için iyi bir yolu, ancak nasıl gerçek web uygulaması oluşturma istemezsiniz gelir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="d0e7a-112">Biz geri sitemizi ziyaret tarayıcılara HTML oluşturmak için daha iyi bir yolu istediğiniz olacak – bir biz daha kolay HTML içeriğini özelleştirmek için şablon dosyalarını nereye kullanabilirsiniz gönderir geri.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="d0e7a-113">Tam olarak görünümleri ne olur.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="d0e7a-114">Şablonu görüntüleme ekleme</span><span class="sxs-lookup"><span data-stu-id="d0e7a-114">Adding a View template</span></span>

<span data-ttu-id="d0e7a-115">Bir görünüm şablonu kullanmak için biz ActionResult döndürmek için HomeController dizin yöntemini değiştirin ve sahip aşağıda gibi View(), dönüş:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="d0e7a-116">Bunun yerine bir sonuç yeniden oluşturmak için "Görünüm" kullanmak istiyoruz, yerine bir dize döndürdü yukarıdaki değişiklik gösterir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="d0e7a-117">Şimdi uygun bir görünüm şablon bizim projeye ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="d0e7a-118">Bunu yapmak için şu metin imleci dizin eylem yönteminin içinde getirin sonra sağ tıklatın ve "Görünümü Ekle" seçin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="d0e7a-119">Bu Görünüm Ekle iletişim kutusunu getirir:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="d0e7a-120">![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d0e7a-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="d0e7a-121">"Görünüm Ekle" iletişim kutusu bize hızla ve kolayca görünüm şablon dosyalarını oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="d0e7a-122">Varsayılan olarak "Görünümü Ekle" iletişim kullanacağı eylem yönteminin eşleşen oluşturmak üzere Görünüm şablonu adı önceden doldurur.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="d0e7a-123">Yukarıdaki "Görünüm Ekle" iletişim kutusu, varsayılan olarak önceden doldurulmuş Görünüm adı olarak "Dizin" sahip, "Görünüm Ekle" bağlam menüsü bizim HomeController İNDİS() eylem yöntemi içinde kullandık olduğundan.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="d0e7a-124">Gerekli değil Bu iletişim kutusundaki seçeneklerden herhangi birini değiştirmek için bu nedenle Ekle düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="d0e7a-125">Biz Ekle düğmesine tıkladığınızda Visual Web Developer \Views\Home dizininde durumlarda klasörü oluşturmak için bize görünüm şablonu önceden var olmayan bir yeni Index.cshtml oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="d0e7a-126">"Index.cshtml" dosya adı ve klasör konumunu önemlidir ve varsayılan ASP.NET MVC adlandırma kuralları izler.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="d0e7a-127">Dizin adı, \Views\Home, HomeController adlı denetleyicisi - eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="d0e7a-128">Görünüm şablonu adı, dizin, görünümün görüntüleme denetleyici eylem yöntemine eşleşir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="d0e7a-129">ASP.NET MVC bize bir görünüme dönmek için bu adlandırma kuralını kullanıyoruz, ad veya konum bir görünüm şablonu açıkça belirtmek zorunda kalmamak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="d0e7a-130">Biz bizim HomeController içinde aşağıdaki gibi kodu yazarken \Views\Home\Index.cshtml görünüm şablon varsayılan olarak oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="d0e7a-131">Visual Web Developer oluşturulan ve şablonu görüntüle "Index.cshtml" biz "Görünümü Ekle" iletişim içinde "Ekle" düğmesini tıklattıktan sonra açılan.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="d0e7a-132">Index.cshtml içeriğini aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="d0e7a-133">Bu görünüm, ASP.NET Web Forms ve ASP.NET MVC önceki sürümlerinde kullanılan Web Forms görünüm altyapısı daha kısa Razor sözdizimi kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="d0e7a-134">Web Forms görünüm altyapısı ASP.NET MVC 3'te hala kullanılabilir, ancak Razor görüntüleme altyapısı ASP.NET MVC geliştirme gerçekten iyi uyduğunu geliştiricilerin çoğu bulma.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="d0e7a-135">İlk üç satırını ViewBag.Title kullanarak sayfa başlığını ayarla.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="d0e7a-136">Biz bu daha ayrıntılı olarak yakında, ancak ilk şimdi güncelleştirmenin nasıl çalıştığı metin başlık metni ara ve sayfasını görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="d0e7a-137">Güncelleştirme &lt;h2&gt; etiketi "Bu olduğu giriş sayfası" aşağıda gösterildiği gibi söyleyin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="d0e7a-138">Bizim yeni metin giriş sayfasında görünür olduğunu uygulama gösterir çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="d0e7a-139">Ortak site öğeler için bir düzen kullanarak</span><span class="sxs-lookup"><span data-stu-id="d0e7a-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="d0e7a-140">Çoğu Web siteleri, birçok sayfalar arasında paylaşılan içeriğe sahip: gezinme, altbilgi, logo görüntüler, stil sayfası başvuruları, vb. Razor görüntüleme altyapısı bu adlı sayfaya kullanarak yönetmenizi kolaylaştırır \_/ görünümler/paylaşılan klasöre bize için otomatik olarak oluşturuldu Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="d0e7a-141">Bu klasör, aşağıda gösterilen içeriğini görüntülemek için çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="d0e7a-142">İçeriği bizim tek bir görünüm tarafından görüntülenen @RenderBody() komut ve dışında görünen istiyoruz herhangi bir ortak içerik eklenebilir \_Layout.cshtml biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="d0e7a-143">Biz, doğrudan yukarıdaki şablon ekleyeceğiz bağlantılar sahip ortak bir üstbilgi bizim giriş sayfasının ve depolama alanına sitesindeki tüm sayfalara vardır, yani bizim MVC müzik deposu isteyeceksiniz @RenderBody() ifadesi.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="d0e7a-144">Stil güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="d0e7a-144">Updating the StyleSheet</span></span>

<span data-ttu-id="d0e7a-145">Boş proje şablonu yalnızca doğrulama iletileri görüntülemek için kullanılan stiller içeren çok kolaylaştırılmış bir CSS dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="d0e7a-146">Bizim Tasarımcısı bazı ek CSS ve görüntülerin görünüm için sitemizi, şimdi de ekleyeceğiz şekilde tanımlamak için sağlamıştır.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="d0e7a-147">Güncelleştirilmiş CSS dosya ve görüntüleri kullanılabilir olan MvcMusicStore Assets.zip içerik dizininin içinde yer alan [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="d0e7a-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="d0e7a-148">Biz Windows Gezgini'nde ikisini birden seçin ve aşağıda gösterildiği gibi Visual Web Developer, içerik klasörüne bizim çözümün bırakın:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="d0e7a-149">Mevcut Site.css dosyanın üzerine yazmak isteyip istemediğinizi onaylamanız istenir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="d0e7a-150">Evet'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="d0e7a-151">İçerik klasörünü, uygulamanızın artık şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="d0e7a-152">Şimdi şimdi uygulamayı çalıştırın ve değişikliklerimizi giriş sayfasında nasıl bakmak.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="d0e7a-153">Şimdi Değiştirilenler gözden geçirin: HomeController'ın dizin eylem yöntemi bulunamadı ve standart bir adlandırma kuralına bizim görünüm şablonu ve ardından olduğundan kodumuza "dönüş View()" olarak adlandırılan olsa bile \Views\Home\Index.cshtmlView şablonu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="d0e7a-154">Giriş sayfası \Views\Home\Index.cshtml görünüm şablonda tanımlı basit bir Hoş Geldiniz iletisi görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="d0e7a-155">Giriş sayfasını kullanarak bizim \_Layout.cshtml şablonu ve Hoş Geldiniz iletisi standart site HTML düzenini içinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="d0e7a-156">Bir Model bilgileri bizim görünüme iletmek için kullanma</span><span class="sxs-lookup"><span data-stu-id="d0e7a-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="d0e7a-157">Yalnızca sabit kodlanmış HTML görüntüleyen bir görünüm şablonu çok ilginç bir web sitesi kolaylaştıracağını değil.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="d0e7a-158">Dinamik bir web sitesi oluşturmak için bunun yerine görünümü şablonlarımız bizim denetleyicisi eylemlerden bilgi geçirmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="d0e7a-159">Model-View-Controller desende uygulamasındaki verileri temsil eden modeli başvurduğu terim nesneleri.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="d0e7a-160">Genellikle, model nesneleri veritabanınızdaki tablolar karşılık gelir, ancak zorunda değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="d0e7a-161">ActionResult döndürmesi denetleyici eylem yöntemlerine model nesnesi görünümüne geçiş yapabilir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="d0e7a-162">Bu, uygun HTML yanıtı oluşturmak için kullanılacak bir yanıt oluşturmak ve bu bilgileri bir görünüm şablonu sonra geçirin için gerekli tüm bilgileri düzgün bir şekilde paketi bir denetleyiciye sağlar.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="d0e7a-163">Bu eylem görerek anlamak, başlayabiliriz en kolayıdır.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="d0e7a-164">Türler ve albümleri bizim deposu içinde temsil etmek için bazı modeli sınıfları ilk oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="d0e7a-165">Bir tarzını sınıfı oluşturarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="d0e7a-166">Projenizin içinde "Modelleri" klasörü sağ tıklatın, "Sınıfı Ekle" seçeneğini seçin ve "Genre.cs" dosya adı.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="d0e7a-167">Daha sonra ortak dizesi Name özelliği, oluşturulan sınıfa ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="d0e7a-168">*Not: durumda merak ediyor, {alın; ayarlayın;} gösterimi yapma C# ' ın otomatik uygulanan özellikler özelliğini kullanın. Bu işlem bize bize yedekleme alanı bildirmek gerek kalmadan bir özellik yararları sağlar.*</span><span class="sxs-lookup"><span data-stu-id="d0e7a-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="d0e7a-169">Ardından, bir başlığı ve bir tarzını özelliğine sahiptir (Album.cs adlı) bir albüm sınıfı oluşturmak için aynı adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="d0e7a-170">Şimdi biz bizim modelden dinamik bilgilerini gösteren görünümleri kullanmak için StoreController değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="d0e7a-171">İstek Kimliği temel alarak bizim albümleri, biz şimdi - Tanıtım amaçlı adlı - varsa, biz görünüm olduğu gibi bilgileri görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="d0e7a-172">Tek bir albümü bilgileri gösterecek şekilde deposu ayrıntıları eylem değiştirerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="d0e7a-173">En üst kısmına "kullanarak" deyimine **StoreControllers** biz albüm sınıfını kullanmak istiyoruz her zaman MvcMusicStore.Models.Album yazın gerek kalmaması MvcMusicStore.Models ad alanı içerecek şekilde sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="d0e7a-174">Bu sınıf "kullanımları" bölümünü artık görünmelidir olarak aşağıda.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="d0e7a-175">Böylece bir dize yerine ActionResult döndürür HomeController'ın dizin yöntemiyle yaptığımız gibi ardından, Ayrıntılar denetleyici eylemi güncelleştiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="d0e7a-176">Şimdi biz albüm nesneyi görünümüne dönmek için mantığı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="d0e7a-177">Daha sonra Bu öğreticide biz verilerini veritabanından – alma ancak için şu anda "veri kukla" kullanacağız başlamak için.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="d0e7a-178">*Not: C# ile tanınmayan varsa var kullanarak bizim albüm değişkeni geç bağlama olduğu anlamına gelir varsayabilir. Doğru değil-C# derleyici tür çıkarımı ne Biz bu albüm albüm türünde belirlemek için değişkeni atama ve böylece biz derleme zamanı denetleme ve Visual Studio kod düzenleyicisini elde yerel albüm değişkeni bir albüm tür olarak derleme göre kullanma destekler.*</span><span class="sxs-lookup"><span data-stu-id="d0e7a-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="d0e7a-179">Şimdi bir HTML yanıtı oluşturmak için bizim albüm kullanan bir görünüm şablonu oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="d0e7a-180">Bunu yapmadan Görünüm Ekle iletişim kutusunda, yeni oluşturulan albüm sınıfı hakkında bilir böylece proje oluşturmamız gereken.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="d0e7a-181">Proje Debug⇨Build MvcMusicStore seçerek menü öğesi oluşturabileceğiniz (ek kredi için Ctrl-Shift-B kısayolu projeyi oluşturmak için kullanabileceğiniz).</span><span class="sxs-lookup"><span data-stu-id="d0e7a-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="d0e7a-182">Biz bizim destekleyen sınıfları ayarladınız, biz bizim görünüm şablonu oluşturmaya hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="d0e7a-183">Ayrıntılar yöntemi içinde sağ tıklatın ve "Görünümü... Ekle" seçin</span><span class="sxs-lookup"><span data-stu-id="d0e7a-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="d0e7a-184">bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="d0e7a-185">Önce ile HomeController yaptığımız gibi yeni bir görünüm şablonu oluşturma olacak.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="d0e7a-186">StoreController oluşturuyoruz çünkü \Views\Store\Index.cshtml dosyasında varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="d0e7a-187">Farklı olarak önce biz "Oluştur kesin türü belirtilmiş bir" görünümü onay alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="d0e7a-188">Biz, ardından "View veri sınıfı" açılır liste içinde bizim "Albümü" sınıf seçmek için çağıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="d0e7a-189">Bu nesne kullanmak için kendisine geçirilen bir albümü, bekliyor bir görünüm şablonu oluşturmak "Ekle" iletişim kutusunu görüntüle neden olur.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="d0e7a-190">Biz "Ekle" düğmesini tıklattığınızda, aşağıdaki kodu içeren bizim \Views\Store\Details.cshtml görünüm şablonu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="d0e7a-191">Bu görünüm kesin türü belirtilmiş-bizim albüm sınıfı gösterir ilk satırda dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="d0e7a-192">Razor görüntüleme altyapısı, böylece biz model özelliklerini kolayca erişmek ve IntelliSense avantajı Visual Web Developer düzenleyicisinde bile sahip, albüm nesneyi geçirildi olduğunu bilir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="d0e7a-193">Güncelleştirme &lt;h2&gt; gibi görünen üzere bu satırı değiştirerek albüm Title özelliğini gösterir şekilde etiketleyin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="d0e7a-194">Süre girdiğinizde, IntelliSense tetiklenir fark @Model anahtar sözcüğü, özellikleri ve albüm sınıfı destekler yöntemleri gösteren.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="d0e7a-195">Şimdi şimdi Projemizin yeniden çalıştırın ve depolama/Ayrıntılar/5 URL'yi ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="d0e7a-196">Aşağıdaki gibi bir albüm ayrıntılarını göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="d0e7a-197">Şimdi biz deposu Gözat eylem yöntemi için benzer bir güncelleştirme yapacağız.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="d0e7a-198">ActionResult döndürecek şekilde güncelleştirme yöntemi ve böylece yeni bir tarzını nesnesi oluşturur ve görünümüne döner yöntemi mantığını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="d0e7a-199">Gözat yönteminde sağ tıklatın ve "Görünümü... Ekle" seçin</span><span class="sxs-lookup"><span data-stu-id="d0e7a-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="d0e7a-200">bağlam menüsünden ardından kesin türü belirtilmiş bir görünüm ekleyin kesin türü belirtilmiş bir tarzını sınıfına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="d0e7a-201">Güncelleştirme &lt;h2&gt; görünümünde öğe Tarz bilgileri görüntülemek için (/Views/Store/Browse.cshtml içinde) kod.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="d0e7a-202">Şimdi şimdi Projemizin yeniden çalıştırın ve Gözat / deposu/için Gözat? Tarz = DISCO URL.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="d0e7a-203">Gibi aşağıda gösterilen gözatma sayfası göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="d0e7a-204">Son olarak, biraz daha karmaşık bir güncelleştirmeye olalım **depolama dizini** eylem yöntemi ve bizim deposunda tüm türler listesini görüntülemek için görünümü.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="d0e7a-205">Yalnızca tek bir tarzını yerine bizim model nesnesi, bir liste türler kullanarak bunu.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="d0e7a-206">Depolama dizini eylem yöntemine sağ tıklatın ve eklemek önce Tarz Model sınıfı seçin ve Ekle düğmesine basın görünümü seçin.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="d0e7a-207">Biz öncelikle değiştireceğiz @model görünümü birkaç Tarz bekleniyor göstermek için bildirim yerine yalnızca bir tanesini nesneleri.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="d0e7a-208">Aşağıdaki şekilde /Store/Index.cshtml ilk satırı değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="d0e7a-209">Bu, birkaç Tarz nesneleri tutabilen bir model nesnesi ile çalışacaksınız Razor görüntüleme altyapısı bildirir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="d0e7a-210">Bir IEnumerable kullanmakta olduğunuz&lt;Tarz&gt; listesini yerine&lt;Tarz&gt; daha genel olduğuna göre bize IEnumerable arabirimini destekleyen herhangi bir nesne türüne bizim model türü daha sonra değiştirmeye izin verme.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="d0e7a-211">Ardından, biz tamamlanmış görünüm kodda gösterildiği gibi modeldeki Tarz nesneleri döngü.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="d0e7a-212">Biz bu kodu girerken biz yazdığınızda böylece biz tam IntelliSense desteği özen gösterin "@Model."</span><span class="sxs-lookup"><span data-stu-id="d0e7a-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="d0e7a-213">Biz, tüm yöntemleri ve özellikleri bir tarzını türü IEnumerable tarafından desteklenen bakın.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="d0e7a-214">Bizim "foreach" döngü içinde Visual Web Developer biz IntelliSence her biri için görmeleri her bir öğe türü Tarz, tarz türü olduğunu olduğunu bilir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="d0e7a-215">Ardından, yapı iskelesi özelliği Tarz nesne incelenmesi ve aracılığıyla döngüler ve bunları Yazar her bir Name özelliğine sahip olma olasılığı belirledi. Ayrıca, her öğeyle düzenleme, Ayrıntılar ve Delete bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="d0e7a-216">Biz, daha sonra Depolama Yöneticisi'nde yararlanmak, ancak şu an için basit bir listesi yerine olmasını isteriz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="d0e7a-217">Uygulamayı çalıştırın ve/Store için Gözat sayısını ve türler listesi görüntülenir bakın.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="d0e7a-218">Sayfaları arasında bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="d0e7a-218">Adding Links between pages</span></span>

<span data-ttu-id="d0e7a-219">Türler şu anda listeler bizim/Store URL yalnızca düz metin olarak Tarz adlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="d0e7a-220">Düz metin yerine bunun yerine uygun deposu/Gözat URL Tarz adları bağlantısını sahibiz böylece bu "Disco" / deposu/için Gözat gideceği gibi bir müzik Tarz tıklayarak böylece değiştirelim? Tarz = DISCO URL.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="d0e7a-221">Bizim \Views\Store\Index.cshtml görünüm şablonu aşağıda kullanarak bu bağlantıları kod gibi çıktıya güncelleştiriyoruz **(Bu tür yok - üzerinde geliştirmek için yapacağız)**:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="d0e7a-222">Çalışan, ancak bir sabit kodlanmış dize kullandığından daha sonra sorun için neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="d0e7a-223">Biz denetleyicisini yeniden adlandırmak istiyorsanız, örneğin, biz güncelleştirilmesi gereken bağlantıları için arayan kodumuza arama yapmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="d0e7a-224">Bir HTML yardımcı yöntem yararlanmak için kullandığımız alternatif bir yaklaşım değil.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="d0e7a-225">ASP.NET MVC bizim görünüm şablonu kodundan çeşitli olduğu gibi genel görevleri gerçekleştirmek için kullanılabilir olan HTML yardımcı yöntemler içerir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="d0e7a-226">Html.ActionLink() yardımcı yöntem özellikle yararlı bir ve HTML oluşturmanızı kolaylaştırır &lt;bir&gt; bağlar ve URL yollarını URL kodlanmış düzgünce emin olmak gibi rahatsız edici ayrıntıları ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="d0e7a-227">Html.ActionLink() bağlantılarınız için gereksinim duyduğunuz kadar çok bilgi belirtme izin vermek için birçok farklı aşırı yüklemeye sahip.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="d0e7a-228">En basit durumda, yalnızca bağlantı metni ve köprü istemcide tıklatıldığında gitmek için bir eylem yönteminin sağlamanız.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="d0e7a-229">Örneğin, biz "/ deposu /" ile bağlantı metni "Gitmek için deposu aşağıdaki çağrıyı kullanarak dizini" deposu Ayrıntıları sayfasında İNDİS() yöntemi bağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="d0e7a-230">*Not: Bu durumda, biz yalnızca geçerli görünüm işlemeyle aynı denetleyicinin içinde başka bir eylem için bağlıyorsanız çünkü denetleyicinin adını belirtmek ihtiyacımız alamadık.*</span><span class="sxs-lookup"><span data-stu-id="d0e7a-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="d0e7a-231">Başka bir aşırı yüklemesini üç parametre alır Html.ActionLink yöntemi kullanacağız şekilde Gözat sayfa bizim bağlanan bir parametre, yine de geçirmek gerekir:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="d0e7a-232">Bağlantı metnini, tarz adını görüntüler</span><span class="sxs-lookup"><span data-stu-id="d0e7a-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="d0e7a-233">Denetleyici eylem adı (Gözat)</span><span class="sxs-lookup"><span data-stu-id="d0e7a-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="d0e7a-234">Rota parametre değerleri belirtme (Tarz) adı ve değeri (Tarz adı),</span><span class="sxs-lookup"><span data-stu-id="d0e7a-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="d0e7a-235">Hepsini bir araya burada olduğunu Biz bu bağlantıları deposu dizini görünümüne nasıl yazacaksınız koyma:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="d0e7a-236">Şimdi sizi Projemizin yeniden çalıştırdığınızda ve /Store/ URL'ye erişim biz türler listesini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="d0e7a-237">Her bir tarzını – köprü tıklatıldığında olan, bize bizim/deposu/için Gözat sürer? Tarz =*[Tarz]* URL.</span><span class="sxs-lookup"><span data-stu-id="d0e7a-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="d0e7a-238">HTML Tarz listesi için şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="d0e7a-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="d0e7a-239">[Önceki](mvc-music-store-part-2.md)
> [sonraki](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="d0e7a-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
