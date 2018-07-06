---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "3. Bölüm: Görünümler ve Viewmodel'lar | Microsoft Docs"
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 3, görünümler ve Viewmodel'lar kapsar.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 8fd89c2a448877bf13a7828f545ffcd400f63bb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837416"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="a1ab3-104">3. Bölüm: Görünümler ve Viewmodel'lar</span><span class="sxs-lookup"><span data-stu-id="a1ab3-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="a1ab3-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a1ab3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a1ab3-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a1ab3-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a1ab3-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a1ab3-109">Bölüm 3, görünümler ve Viewmodel'lar kapsar.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="a1ab3-110">Şu ana kadar biz yalnızca dizeleri denetleyici eylemlerine döndürmeyi.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="a1ab3-111">Denetleyicileri nasıl çalıştığı hakkında fikir almak için iyi bir yolu olan ancak olduğu nasıl gerçek bir web uygulaması derleme istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="a1ab3-112">Daha iyi bir yolu geri sitemizi ziyaret tarayıcılara HTML oluşturmak istediğiniz kullanacağız: geri bir kolayca HTML içeriğini özelleştirmek için şablon dosyaları burada kullanabiliriz gönderin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="a1ab3-113">Tam olarak neler görünümleri olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="a1ab3-114">Bir görünüm şablonu ekleme</span><span class="sxs-lookup"><span data-stu-id="a1ab3-114">Adding a View template</span></span>

<span data-ttu-id="a1ab3-115">Görünüm şablonu kullanmak için biz ActionResult döndürülecek HomeController dizin yöntemini değiştirin ve sahip aşağıdaki gibi View(), dönüş:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="a1ab3-116">Yukarıdaki değişikliği yerine bir dize döndürdüğünü gösterir, bunun yerine bir sonucu geri üretmek için "Görünüm" kullanılacak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="a1ab3-117">Artık uygun bir şablonu görüntüleme için Projemizin ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="a1ab3-118">Bunu yapmak için biz dizin eylem yöntemi içinde metin imleci konumlandırma sonra sağ tıklayın ve "Görünüm Ekle"'i seçin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="a1ab3-119">Bu Görünüm Ekle iletişim kutusu getirir:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="a1ab3-120">![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1ab3-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="a1ab3-121">"Görünüm Ekle" iletişim kutusu hızlı ve kolay bir görünümü şablon dosyaları oluşturmasını sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="a1ab3-122">Varsayılan olarak "Görünüm Ekle" iletişim kullanacak olan eylem yönteminin eşleşmesi oluşturmak için Görünüm şablonunun adı önceden doldurur.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="a1ab3-123">Bizim HomeController İNDİS() eylem yöntemi içindeki "Görünüm Ekle" bağlam menüsü kullandığımız için yukarıdaki "Görünüm Ekle" iletişim kutusu "Index" Görünüm adını varsayılan olarak önceden doldurulmuş olarak sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="a1ab3-124">Gerekmez bu iletişim kutusundaki seçeneklerden herhangi birini değiştirmek için bu nedenle Ekle düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="a1ab3-125">Biz Ekle düğmesine tıkladığınızda, Visual Web Developer \Views\Home dizininde değilse klasör oluşturma görünümü şablon bizim için zaten yoksa yeni bir Index.cshtml oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="a1ab3-126">"Index.cshtml" dosya adı ve klasör konumunu önemlidir ve varsayılan ASP.NET MVC adlandırma kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="a1ab3-127">Dizin adı, \Views\Home, HomeController adlı denetleyicisi - eşleşir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="a1ab3-128">Görünüm şablonu adı, dizin, görünümün görüntüleme denetleyici eylem yöntemine eşleşir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="a1ab3-129">ASP.NET MVC görünüm döndürmek için bu adlandırma kuralı kullanıyoruz, adına veya konumuna bir görünüm şablonunun açıkça belirtmek zorunda kalmamak olanak sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="a1ab3-130">Size sunduğumuz HomeController içinde aşağıdaki gibi kod yazdığınızda, varsayılan olarak \Views\Home\Index.cshtml görünüm şablonu işlenir:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="a1ab3-131">Visual Web Developer, oluşturulan ve size "Görünüm Ekle" iletişim kutusu içinde "Ekle" düğmesine tıkladı sonra "Index.cshtml" Görünüm şablonu açılır.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="a1ab3-132">Index.cshtml içeriğini aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="a1ab3-133">Bu görünüm, Web Forms, ASP.NET Web Forms ve ASP.NET MVC önceki sürümlerinde kullanılan görünüm altyapısını daha kısa olan Razor sözdizimi kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="a1ab3-134">Web Forms görünüm altyapısı ASP.NET MVC 3'te hala kullanılabilir, ancak Razor görüntüleme motorunu ASP.NET MVC geliştirme gerçekten iyi uyduğunu birçok geliştiricinin bulun.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="a1ab3-135">İlk üç satırını ViewBag.Title kullanarak sayfa başlığını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="a1ab3-136">Biz bunun daha ayrıntılı olarak yakında, ancak ilk şimdi güncelleştirme işleyişi sırasında Başlık metin ara ve sayfayı görüntülemek.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="a1ab3-137">Güncelleştirme &lt;h2&gt; etiketi aşağıda gösterildiği gibi "Bu olduğundan ana sayfa" deyin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="a1ab3-138">Sunduğumuz yeni metin giriş sayfasında görünür olduğunu uygulama programları çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="a1ab3-139">Sık kullanılan site öğeleri bir düzen kullanarak</span><span class="sxs-lookup"><span data-stu-id="a1ab3-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="a1ab3-140">Çoğu Web sitesi birçok sayfalar arasında paylaşılan içeriğe sahip: gezinti, altbilgiler, logosu görüntüler, stil sayfası başvuruları, vs. Razor görünüm altyapısı bu adlı bir sayfasını kullanarak yönetmenizi kolaylaştırır \_Layout.cshtml, / görünümler/paylaşılan klasörü içinde bizim için otomatik olarak oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="a1ab3-141">Bu klasör ve aşağıda da gösterilen içeriği görüntülemek için çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="a1ab3-142">İçeriği bizim tek bir görünüm tarafından görüntülenen @RenderBody() komut ve dışında görünen istediğiniz herhangi bir ortak içerik eklenebilir \_Layout.cshtml biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="a1ab3-143">Biz, şablona doğrudan yukarıdaki ekleyeceğiz şekilde bizim giriş sayfası ve Store alanına sitesindeki tüm sayfalara bağlantılarla birlikte ortak bir üst bilgi sağlamak için sunduğumuz MVC müzik Store isteyeceksiniz @RenderBody() deyimi.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="a1ab3-144">Stil sayfası güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="a1ab3-144">Updating the StyleSheet</span></span>

<span data-ttu-id="a1ab3-145">Boş proje şablonu, yalnızca doğrulama iletileri görüntülemek için kullanılan stilleri içeren çok kolaylaştırılmış bir CSS dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="a1ab3-146">Bizim Tasarımcısı, bazı ek CSS ve görüntüleri görünümü için sitemizi, şimdi de ekleyeceğiz şekilde tanımlamak için sağlamıştır.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="a1ab3-147">Güncelleştirilmiş CSS dosyası ve görüntü kullanılabilir olan MvcMusicStore Assets.zip içerik dizinin içinde yer [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="a1ab3-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="a1ab3-148">Biz ikisini birden Windows Gezgini'nde seçin ve aşağıda gösterildiği gibi bizim çözümün Visual Web Developer, içerik klasörüne bırakın:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="a1ab3-149">Mevcut Site.css dosyanın üzerine yazmak isteyip istemediğinizi onaylamanız istenir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="a1ab3-150">Evet'e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="a1ab3-151">Uygulamanızın içerik klasörünü artık şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="a1ab3-152">Şimdi uygulamayı çalıştırabilir ve yaptığımız değişiklikleri giriş sayfasında nasıl göründüğünü görmek şimdi.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="a1ab3-153">Nelerin değiştiğini gözden geçirelim: HomeController'ın dizin eylem yöntemi bulunamadı ve bizim görünüm şablonu standart bir adlandırma kuralını izleyen çünkü kodumuz "dönüş View()" adlı olsa bile \Views\Home\Index.cshtmlView şablonu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="a1ab3-154">Giriş sayfası \Views\Home\Index.cshtml görünüm şablonu içinde tanımlanan basit bir Hoş Geldiniz iletisi görüntülüyor.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="a1ab3-155">Giriş sayfasını kullanarak bizim \_Layout.cshtml şablonu ve HTML standart site düzenine Hoş Geldiniz iletisi bulunur.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="a1ab3-156">Bilgi bizim görünüme iletmek için bir modeli kullanma</span><span class="sxs-lookup"><span data-stu-id="a1ab3-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="a1ab3-157">Sabit kodlanmış HTML yalnızca görüntüleyen bir görünüm şablonu çok ilginç bir web sitesi yapacağını değil.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="a1ab3-158">Dinamik bir web sitesi oluşturun, biz bunun yerine görünümü şablonlarımızı denetleyicisi eylemlerimiz bilgi geçirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="a1ab3-159">Model-View-Controller desende, uygulamadaki verileri temsil modeli başvurduğu terimi nesneleri.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="a1ab3-160">Genellikle, model nesneleri, veritabanınızdaki tablolar karşılık gelir, ancak gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="a1ab3-161">ActionResult döndürmesi denetleyici eylem yöntemlerinde görünümü için bir model nesnesi geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="a1ab3-162">Bu, uygun HTML yanıtı oluşturmak için kullanılacak bir yanıt oluşturur ve ardından bir görünüm şablonu için bu bilgileri geçirin için gerekli tüm bilgileri indrebilirsiniz paketi bir denetleyiciye sağlar.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="a1ab3-163">Bu eylem görerek anlamak, başlayabiliriz en kolay yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="a1ab3-164">Türleri ve albümleri mağazamız içinde temsil etmek için bazı Model sınıfları ilk oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="a1ab3-165">Bir türe sınıfı oluşturarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="a1ab3-166">Projeniz içindeki "Modelleri" klasörü sağ tıklatın, "Sınıf Ekle" seçeneğini belirleyin ve "Genre.cs" dosya adı.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="a1ab3-167">Daha sonra oluşturulan sınıfa genel bir dize adı özelliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="a1ab3-168">*Not: durumda merak ediyor, {alın; ayarlayın;} gösterimi yapma, C# ' nin otomatik olarak uygulanan özellikler özelliğinin kullanımını. Bu işlem ABD bize destek alanı bildirmek gerek kalmadan özellik avantajlarını sağlar.*</span><span class="sxs-lookup"><span data-stu-id="a1ab3-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="a1ab3-169">Ardından, bir başlık ve bir türe özelliğine sahiptir (Album.cs adlı) bir albüm sınıfı oluşturmak için aynı adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="a1ab3-170">Şimdi biz Modelimizi dinamik bilgileri gösteren görünümleri kullanmayı StoreController değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="a1ab3-171">İstek Kimliği temel alarak bizim albümleri, biz şu anda - tanıtım amacıyla adlı - varsa, bu bilgileri aşağıdaki görünüm olduğu gibi görüntüleriz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="a1ab3-172">Store Ayrıntılar eylemi için tek bir albümü bilgileri gösterecek şekilde değiştirerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="a1ab3-173">En üst kısmına "kullanarak" deyimine **StoreControllers** biz albüm sınıfını kullanmak istediğiniz her seferinde MvcMusicStore.Models.Album yazmak zorunda kalmazsınız MvcMusicStore.Models ad alanı içerecek şekilde sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="a1ab3-174">Bu sınıfın "kullanımları" bölümü artık görünmesi gereken aşağıdaki gibi.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="a1ab3-175">Böylece bir dize yerine ActionResult döndürür HomeController'ın dizin yöntemiyle yaptığımız gibi ardından, Ayrıntılar denetleyici eylemi güncelleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="a1ab3-176">Şimdi biz albüm nesnenin görünümüne dönmek için mantığı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="a1ab3-177">Bu öğreticinin ilerleyen bölümlerinde biz veriler bir veritabanından – alınıyor, ancak şu an için "veri kukla" kullanacağız kullanmaya başlamak için.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="a1ab3-178">*Not: C# ile ilk kez kullanıyorsanız var kullanarak albüm değişkeni geç bağlanan anlamına varsayabilir. Doğru değil-C# derleyici tür çıkarımı ne biz albüm türüdür, albüm belirlemek için bir değişkene atayarak ve derleme zamanı denetimi ve Visual Studio Kod Düzenleyicisi aldığımız için yerel albüm değişkeni albüm türü olarak, derleme göre kullanıyor destekler.*</span><span class="sxs-lookup"><span data-stu-id="a1ab3-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="a1ab3-179">Artık bir HTML yanıtı oluşturmak için sunduğumuz albüm kullanan bir görünüm şablonu oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="a1ab3-180">Bunu yapmadan önce projeyi derleyin, Görünüm Ekle iletişim kutusunda yeni oluşturulan bizim albüm sınıfı hakkında bilebilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="a1ab3-181">Proje Debug⇨Build MvcMusicStore seçerek menü öğesi oluşturabileceğinizi (götürmek için Ctrl-Shift-B kısayolu Projeyi derlemek için kullanabileceğiniz).</span><span class="sxs-lookup"><span data-stu-id="a1ab3-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="a1ab3-182">Size sunduğumuz destekleyen sınıfları ayarladınız, bizim görünüm şablonu oluşturmak hazırız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="a1ab3-183">Ayrıntılar yöntemi içinde sağ tıklayın ve bağlam menüsünden "Görünümü Ekle..."'i seçin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="a1ab3-184">Önce HomeController ile yaptığımız gibi yeni bir görünüm şablonu oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="a1ab3-185">StoreController oluşturuyoruz çünkü \Views\Store\Index.cshtml dosyasında varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="a1ab3-186">Aksine, önce "Oluşturma türü kesin belirlenmiş bir" görünümü onay kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="a1ab3-187">Ardından, "Albümü" sınıfı "Görünüm veri class" açılır liste içinde kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="a1ab3-188">Bu, bekliyor bir görünüm şablonu kullanmak için nesne geçirilir albüm oluşturmak "Görünüm Ekle" iletişim neden olur.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="a1ab3-189">Biz "Ekle" düğmesine tıkladığınızda, aşağıdaki kodu içeren bizim \Views\Store\Details.cshtml görünüm şablonu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="a1ab3-190">Bu görünüm, bizim albüm sınıfı için kesin olduğunu gösteren ilk satırda dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="a1ab3-191">Razor görüntüleme motorunu, biz model özelliklerini kolayca erişin ve hatta Visual Web Developer düzenleyicisindeki IntelliSense avantajı vardır, albüm nesneyi geçirildi olduğunu anlar.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="a1ab3-192">Güncelleştirme &lt;h2&gt; bu satırı aşağıdaki gibi görünen değiştirerek albüm başlık özelliğini görüntüleyecek şekilde etiketleyin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="a1ab3-193">Sonra nokta girdiğinizde, IntelliSense tetiklenen fark @Model anahtar sözcüğü, özellikleri ve yöntemleri albüm sınıfı desteklediği gösteriliyor.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="a1ab3-194">Şimdi artık Projemizin yeniden çalıştırın ve 5/Ayrıntılar/Store URL'sini ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="a1ab3-195">Gibi bir albümü ayrıntılarını göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="a1ab3-196">Artık Store Gözat eylem yöntemine benzer bir güncelleştirme oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="a1ab3-197">ActionResult döndürecek şekilde güncelleştirme yöntemi ve yeni bir türe nesnesi oluşturur ve görünümü döndürür yöntemi mantığını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="a1ab3-198">Göz atma yönteminde sağ tıklayın ve bağlam menüsünden "Görünümü Ekle..." türü kesin belirlenmiş bir görünüm eklersiniz türü kesin belirlenmiş Tarz sınıfına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="a1ab3-199">Güncelleştirme &lt;h2&gt; öğesi Görünümü'nde (/Views/Store/Browse.cshtml içinde) Tarz bilgileri görüntülemek için kod.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="a1ab3-200">Şimdi şimdi Projemizin yeniden çalıştırın ve Gözat / Store/dizinine göz atma? Tarz = DISCO URL.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="a1ab3-201">Aşağıda gösterilen gibi göz atma sayfasından göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="a1ab3-202">Son olarak, biraz daha karmaşık bir güncelleştirme olalım **Store dizini** eylem yöntemi ve bizim deposunda tüm türleri listesini görüntülemek için görünümü.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="a1ab3-203">Biz bu, bir liste türleri yalnızca tek bir türe yerine bizim model nesnesi kullanarak gerçekleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="a1ab3-204">Store dizini eylem yönteminde sağ tıklatın ve eklemek daha önce tarzı Model sınıfı seçin ve Ekle düğmesine basın görünümü seçin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="a1ab3-205">İlk değiştireceğiz @model görünümü birkaç Tarz bekleniyor belirtmek için bildirimi yerine yalnızca bir tane nesneleri.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="a1ab3-206">Şu şekilde okunacak /Store/Index.cshtml ilk satırı değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="a1ab3-207">Bu, birkaç Tarz nesnesi tutabilen bir model nesnesi ile çalışacaksınız Razor görünüm altyapısı bildirir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="a1ab3-208">Bir IEnumerable kullanıyoruz&lt;Tarz&gt; bir listesi yerine&lt;Tarz&gt; daha genel olduğundan, daha sonra model türü IEnumerable arabirimini destekleyen herhangi bir nesne türü değiştirmek bize verme.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="a1ab3-209">Ardından, tamamlanmış görünüm kodda gösterildiği gibi modelinde Tarz nesneler aracılığıyla döngüye gireceğiz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="a1ab3-210">Biz bu kodun girerken biz yazdığınızda emin ediyoruz tam IntelliSense desteği olduğunu fark edeceksiniz "@Model."</span><span class="sxs-lookup"><span data-stu-id="a1ab3-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="a1ab3-211">tüm yöntemleri ve özellikleri Tarz türünde bir IEnumerable tarafından desteklenen görüyoruz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="a1ab3-212">Bizim "foreach" döngü içinde IntelliSence her biri için görüyoruz her öğenin tür Tarz, tarz türü olduğundan, Visual Web Developer bilir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="a1ab3-213">Ardından, iskele kurma özelliği Tarz nesne incelenir ve aracılığıyla döngüye girer ve bunları yazıyor her bir Name özelliğine sahip olacağını belirler. Ayrıca, her bir öğe Düzenle, Ayrıntılar ve Sil bağlantılarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="a1ab3-214">Biz, daha sonra Depolama Yöneticisi'nde yararlanmak, ancak şu an için basit bir listesi yerine olmasını istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="a1ab3-215">Uygulamayı çalıştırmak ve/Store için Gözat sayısı ve türleri listesi görüntülenir bakın.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="a1ab3-216">Sayfaları arasında bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="a1ab3-216">Adding Links between pages</span></span>

<span data-ttu-id="a1ab3-217">Türleri şu anda listeleyen/Store URL'nin yalnızca düz metin olarak Tarz adlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="a1ab3-218">Düz metin yerine biz bunun yerine ilgili Store/göz atma URL Tarz adları bağlantısını edinecek olmanızdır bu "Disco" Store/için Gözat gider gibi bir müzik Tarz tıklayarak böylece değiştirelim? Tarz = DISCO URL.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="a1ab3-219">Biz aşağıdaki gibi kullanarak bu bağlantıları kod çıktısına bizim \Views\Store\Index.cshtml görünüm şablonu güncelleştirebilir **(Bu tür yok - üzerinde geliştirmek için yapacağız)**:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="a1ab3-220">Çalışan, ancak bunu daha sonra bir sabit kodlanmış dizesine kullandığından sorun neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="a1ab3-221">Örneği için denetleyici yeniden adlandırmak istedik, biz güncelleştirilmesi gereken bağlantıları için arayan kodumuz arama gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="a1ab3-222">Bir HTML yardımcı yöntem yararlanmak için kullanabiliriz alternatif bir yaklaşım olan.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="a1ab3-223">ASP.NET MVC görünüm şablonu kodumuz çeşitli olduğu gibi ortak görevleri gerçekleştirmek için kullanılabilir olan HTML yardımcı yöntemler içerir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="a1ab3-224">Html.ActionLink() yardımcı yöntemi, özellikle yararlı bir ve HTML oluşturmayı kolaylaştırır &lt;bir&gt; bağlar ve URL yolları URL kodlanmış düzgün olduğundan emin olmak gibi rahatsız edici ayrıntılarının üstlenir.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="a1ab3-225">Html.ActionLink() bağlantılarınız için gereksinim duyduğunuz kadar çok bilgi belirtilmesine izin verecek şekilde birçok farklı aşırı yüklemeye sahip.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="a1ab3-226">En basit durumda, yalnızca bağlantı metni ve istemcide köprü tıklandığında gitmek için bir eylem yönteminin verin.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="a1ab3-227">Örneğin, biz "/ Store /" ile bağlantı metni "Gitmek için Store çağrısını kullanarak dizin" Store ayrıntıları sayfasındaki İNDİS() yöntemi bağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="a1ab3-228">*Not: Bu durumda, biz size yalnızca geçerli görünüm işlemeyle aynı denetleyici içinde başka bir eylem bağlantı kurduğunuz çünkü Denetleyici adı belirtmeniz gerekmediğine.*</span><span class="sxs-lookup"><span data-stu-id="a1ab3-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="a1ab3-229">Başka bir aşırı üç parametre almayan Html.ActionLink yöntemin kullanacağız. Bu nedenle bizim bağlantılar göz atma sayfasından bir parametre, yine de iletmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="a1ab3-230">Bağlantı metnini, tarzı adı görüntülenir</span><span class="sxs-lookup"><span data-stu-id="a1ab3-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="a1ab3-231">Denetleyici eylem adı (göz atma)</span><span class="sxs-lookup"><span data-stu-id="a1ab3-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="a1ab3-232">Rota parametre değerleri, hem ' % s'adı (Tarz) hem de ' % s'değeri (Tarz adı) belirtme</span><span class="sxs-lookup"><span data-stu-id="a1ab3-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="a1ab3-233">Tümünü bir araya burada emin ediyoruz Store dizini görünümü için bu bağlantıları nasıl yazacaksınız sokarak:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="a1ab3-234">Şimdi, projeyi yeniden çalıştırın ve /Store/ URL'sine erişin türleri listesini göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="a1ab3-235">Her bir tür – köprü tıklandığında olan bize bizim/Store/dizinine göz atma sürer? Tarz =*[Tarz]* URL'si.</span><span class="sxs-lookup"><span data-stu-id="a1ab3-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="a1ab3-236">HTML Tarz listesi için şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="a1ab3-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="a1ab3-237">[Önceki](mvc-music-store-part-2.md)
> [İleri](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="a1ab3-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
