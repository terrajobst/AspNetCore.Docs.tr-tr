---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: '5. Kısım: Formları düzenle ve şablon | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 5 formları düzenle ve şablon kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874918"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="47b8e-104">5. Kısım: Formları düzenle ve şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="47b8e-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="47b8e-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="47b8e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="47b8e-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="47b8e-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="47b8e-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="47b8e-109">Bölüm 5 formları düzenle ve şablon kapsar.</span><span class="sxs-lookup"><span data-stu-id="47b8e-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="47b8e-110">Önceki bölümde, biz bizim veritabanından veri yükleme ve onu görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="47b8e-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="47b8e-111">Bu bölümde, biz de verileri düzenleme etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="47b8e-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="47b8e-112">StoreManagerController oluşturma</span><span class="sxs-lookup"><span data-stu-id="47b8e-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="47b8e-113">Biz adlı yeni bir denetleyici oluşturarak başlarsınız **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="47b8e-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="47b8e-114">Bu denetleyici için biz ASP.NET MVC 3 araçları güncelleştirme kullanılabilir yapı İskelesi özelliklerinden yararlanma.</span><span class="sxs-lookup"><span data-stu-id="47b8e-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="47b8e-115">Aşağıda gösterildiği gibi denetleyici Ekle iletişim kutusu için seçenekleri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="47b8e-116">Ekle düğmesine tıkladığınızda, ASP.NET MVC 3 yapı iskelesi mekanizması iyi tutar iş sizin için yapar olduğunu görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="47b8e-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="47b8e-117">Yeni StoreManagerController sahip yerel bir Entity Framework değişken oluşturur</span><span class="sxs-lookup"><span data-stu-id="47b8e-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="47b8e-118">Projenin görünümleri klasörüne StoreManager klasör ekler</span><span class="sxs-lookup"><span data-stu-id="47b8e-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="47b8e-119">Albüm sınıfına kesin türü belirtilmiş Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml ve Index.cshtml görünümü ekler</span><span class="sxs-lookup"><span data-stu-id="47b8e-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="47b8e-120">CRUD yeni StoreManager denetleyicisi sınıfı içerir (oluşturma, okuma, güncelleştirme, silme) albümü ile çalışmaya nasıl bilmeniz denetleyici eylemleri model sınıfı ve veritabanı erişimi için bizim Entity Framework bağlamına kullanın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="47b8e-121">Kurulmuş bir görünümü değiştirme</span><span class="sxs-lookup"><span data-stu-id="47b8e-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="47b8e-122">Bu kod bize oluşturuldu, ancak yalnızca biz Bu öğreticide yazma gibi standart ASP.NET MVC kod olduğundan, unutmamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="47b8e-123">Demirbaş denetleyicisi kod yazma ve el ile güçlü şekilde yazılan görünümler oluşturma harcadığı zamanı kaydetmek hedeflenen, ancak bu tür nasıl değiştiğini dpm'nin ilgili açıklama doğrudan uyarılarla başında oluşturulan kodu görülen değil kod.</span><span class="sxs-lookup"><span data-stu-id="47b8e-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="47b8e-124">Bu, kodunuzu ve değiştirmek için bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="47b8e-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="47b8e-125">Bu nedenle, StoreManager dizin görüntülemek için hızlı bir düzenleme ile başlayalım (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="47b8e-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="47b8e-126">Bu görünüm bizim deposundaki albümleri düzenleyin listeleyen bir tablo görüntüler / Ayrıntılar / silme bağlantılar ve albüm genel özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="47b8e-127">Bu görünümde çok yararlı olmadığından biz AlbumArtUrl alan kaldırırsınız.</span><span class="sxs-lookup"><span data-stu-id="47b8e-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="47b8e-128">İçinde &lt;tablo&gt; bölüm görünümü kodunu, kaldırma &lt;th&gt; ve &lt;td&gt; AlbumArtUrl başvuruları aşağıda vurgulanan satırlar tarafından belirtildiği şekilde çevreleyen öğeleri:</span><span class="sxs-lookup"><span data-stu-id="47b8e-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="47b8e-129">Değiştirilen görünümü kod şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="47b8e-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="47b8e-130">Mağaza Yöneticisi ilk bakma</span><span class="sxs-lookup"><span data-stu-id="47b8e-130">A first look at the Store Manager</span></span>

<span data-ttu-id="47b8e-131">Şimdi uygulamayı çalıştırın ve/StoreManager/göz atın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="47b8e-132">Bu, Depolama Yöneticisi biz yalnızca, bağlantılar, ayrıntı, düzenleme ve silme ile deposundaki albümleri listesini gösteren değiştiren dizin görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="47b8e-133">Düzenleme bağlantısını tıklatarak alanları düzenleme formla tarz ve sanatçı bırakmalar de dahil olmak üzere, albüm görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="47b8e-134">Altındaki "Geri listeye" bağlantısını tıklatın ve bir Albüm Ayrıntıları bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="47b8e-135">Bu, tek tek albüm için ayrıntılı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="47b8e-136">Yeniden listesi bağlamak için Geri'yi tıklatın, sonra bir Delete bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="47b8e-137">Bu, albüm ayrıntılarını gösteren ve biz biz silmek istediğinizden emin olup olmadığınızı soran bir onay iletişim kutusunda, görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="47b8e-138">Altındaki Sil düğmesini tıklatarak albümü silin ve silinmiş albümü gösterir dizin sayfasına dönün.</span><span class="sxs-lookup"><span data-stu-id="47b8e-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="47b8e-139">Mağaza Yöneticisi ile bitmek değil, ancak denetleyici ve görünüm kod başlangıcı CRUD işlemleri için çalışma vardır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="47b8e-140">Mağaza Yöneticisi denetleyicisi koda bakmak</span><span class="sxs-lookup"><span data-stu-id="47b8e-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="47b8e-141">Mağaza Yöneticisi denetleyicisi iyi miktarda kod içerir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="47b8e-142">Bu işlemlerde üstten alta edelim.</span><span class="sxs-lookup"><span data-stu-id="47b8e-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="47b8e-143">Bazı standart ad alanları bizim modelleri ad alanı referansı yanı sıra, MVC denetleyicisi için denetleyici içerir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="47b8e-144">Denetleyici MusicStoreEntities, her denetleyici eylemleri tarafından kullanılan veri erişim için özel bir örneğine sahip.</span><span class="sxs-lookup"><span data-stu-id="47b8e-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="47b8e-145">Yöneticisi dizin ve ayrıntıları eylemlerini depolama</span><span class="sxs-lookup"><span data-stu-id="47b8e-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="47b8e-146">Dizin görünümünün albümleri, daha önce deposu Gözat yöntemi çalışırken gördüğümüz gibi her albüm başvurulan tarz ve sanatçı bilgileri de dahil olmak üzere bir listesini alır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="47b8e-147">Denetleyici verimli ve özgün istekteki bu bilgiler için sorgulama için her albüm Tarz adı ve sanatçı adı görüntüleyebilmesi dizin görünümünün bağlı nesnelere başvurular takip ediyor.</span><span class="sxs-lookup"><span data-stu-id="47b8e-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="47b8e-148">StoreManager denetleyicinin ayrıntıları denetleyici eylemi tam olarak aynı yazdığımız daha önce - albüm için sorgular deposu Denetleyicisi ayrıntıları eylem olarak çalışır Find() yöntemini kullanarak Kimliğine göre sonra görünümüne döndürür.</span><span class="sxs-lookup"><span data-stu-id="47b8e-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="47b8e-149">Eylem yöntemleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="47b8e-149">The Create Action Methods</span></span>

<span data-ttu-id="47b8e-150">Form girişi işlediğinden Create eylem yöntemlerini o ana kadarki gördük olanları biraz farklıdır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="47b8e-151">Bir kullanıcı ilk /StoreManager/Oluştur/ziyaret ettiğinde boş bir form gösterilir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="47b8e-152">Bu HTML sayfası içerecek bir &lt;form&gt; açılır ve metin kutusu içeren öğe girişi burada bunlar girebilirsiniz Albüm Ayrıntıları öğeleri.</span><span class="sxs-lookup"><span data-stu-id="47b8e-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="47b8e-153">Kullanıcı albüm form değerlerine doldurur sonra bunlar uygulamamız veritabanı içinde kaydetmek için bu değişiklikleri geri göndermek için "Kaydet" düğmesine basabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47b8e-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="47b8e-154">Kullanıcı "Kaydet" düğmesine bastığında &lt;form&gt; geri /StoreManager/Oluştur/URL için bir HTTP POST gerçekleştirmek ve gönderme &lt;form&gt; değerleri HTTP POST bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="47b8e-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="47b8e-155">ASP.NET MVC bize bize bizim StoreManagerController sınıfı içinde – /StoreManager/Create ilk HTTP GET Gözat işlemek için iki ayrı "Oluştur" eylem yöntemleri uygulamak etkinleştirerek bu iki URL çağırma senaryolar mantığı oluşturan kolayca bölüneceği sağlar / URL ve diğer gönderilen değişiklikleri HTTP POST işlemek için.</span><span class="sxs-lookup"><span data-stu-id="47b8e-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="47b8e-156">Görünüm paketi kullanarak bir görünüme bilgi geçirme</span><span class="sxs-lookup"><span data-stu-id="47b8e-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="47b8e-157">Biz ViewBag Bu öğreticide daha önce kullandıysanız, ancak çok ilgili açıklandı henüz.</span><span class="sxs-lookup"><span data-stu-id="47b8e-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="47b8e-158">Görünüm Paketi bize bilgi kesin türü belirtilmiş model nesnesi kullanmadan görünüme iletmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="47b8e-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="47b8e-159">Bu durumda, bizim Düzenle HTTP GET denetleyici eylemi forma bırakmalar doldurmak için her iki tür ve listesini Sanatçılar geçirmek gerekir ve bunu yapmanın en kolay yolu ViewBag öğeleri olarak döndürülecek.</span><span class="sxs-lookup"><span data-stu-id="47b8e-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="47b8e-160">Görünüm paketini bir dinamik Nesne ViewBag.Foo veya ViewBag.YourNameHere bu özellikleri tanımlamak için kod yazmadan yazabilirsiniz olduğunu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="47b8e-161">Bu durumda, form ile gönderilen açılır değerler bunlar ayarlamak albümü özellikleri GenreId ve ArtistId, böylece denetleyicisi kodu ViewBag.GenreId ve ViewBag.ArtistId kullanır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="47b8e-162">Bu açılır liste değerleri için yalnızca bu amaçla oluşturulmuş SelectList nesnesini kullanarak form döndürülür.</span><span class="sxs-lookup"><span data-stu-id="47b8e-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="47b8e-163">Bu gerçekleştirilir gibi bu kodu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="47b8e-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="47b8e-164">Eylem yöntemi koddan gördüğünüz gibi bu nesnenin oluşturulacağı üç parametre kullanılıyor:</span><span class="sxs-lookup"><span data-stu-id="47b8e-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="47b8e-165">Aşağı açılır görüntüleme öğeleri listesi.</span><span class="sxs-lookup"><span data-stu-id="47b8e-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="47b8e-166">Bu yalnızca bir dize değil - türler listesini geçirme unutmayın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="47b8e-167">SelectList geçirilen sonraki parametresi seçili bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="47b8e-168">Bu önceden listesindeki bir öğeyi seçmek nasıl SelectList nasıl bilir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="47b8e-169">Bu, biz oldukça benzer düzenleme formu baktığınızda anlamak daha kolay olacaktır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="47b8e-170">Son parametre görüntülenecek özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="47b8e-171">Bu durumda, bu Genre.Name özelliği kullanıcıya gösterilecek olduğunu belirten.</span><span class="sxs-lookup"><span data-stu-id="47b8e-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="47b8e-172">Aklınızda ile daha sonra HTTP GET oluşturma eylem oldukça basittir - iki SelectLists ViewBag eklenir ve (bunu henüz oluşturulmamıştır bu yana) hiçbir model nesnesi forma geçirilir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="47b8e-173">HTML Yardımcıları Bırakma listeleri oluşturma görüntülemek için</span><span class="sxs-lookup"><span data-stu-id="47b8e-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="47b8e-174">Biz nasıl değerleri aşağı açılan geçirilir görünümüne hakkında açıklandı olduğundan, bu değerlerin nasıl görüntüleneceğini görmek için görünümü hızlı bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="47b8e-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="47b8e-175">Görünüm kodda (/ Views/StoreManager/Create.cshtml), aşağıdaki çağrıyı Tarz açılır görüntülenecek yapıldığında görürsünüz aşağı.</span><span class="sxs-lookup"><span data-stu-id="47b8e-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="47b8e-176">Bu, bir HTML Yardımcısı - ortak bir görünüm görevi gerçekleştiren bir yardımcı program yöntemi bilinir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="47b8e-177">HTML Yardımcıları kısa ve okunabilir bizim görünümü kodu tutma çok yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="47b8e-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="47b8e-178">Html.DropDownList yardımcı ASP.NET MVC tarafından sağlanır, ancak daha sonra göreceğiz bizim uygulamada yeniden görünümü kodu için kendi Yardımcıları oluşturmak mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="47b8e-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="47b8e-179">Html.DropDownList çağrısı, yalnızca iki şey - get görüntülemek için listede ve (varsa) hangi değer önceden seçilmiş olmalıdır burada söylediyse gerekir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="47b8e-180">İlk parametresi, GenreId, model veya ViewBag GenreId adlı bir değeri aramak için DropDownList bildirir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="47b8e-181">İkinci parametre olarak başlangıçta aşağı açılan listeden seçilen göstermek için değeri belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="47b8e-182">Bu form bir form oluştur olduğundan, hiçbir değer seçilmiş olması ve String.Empty geçirilir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="47b8e-183">Gönderilen Form değerleri işleme</span><span class="sxs-lookup"><span data-stu-id="47b8e-183">Handling the Posted Form values</span></span>

<span data-ttu-id="47b8e-184">Biz önce anlatıldığı gibi her bir formla ilişkili iki eylem yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="47b8e-185">İlk HTTP GET isteği işler ve formu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="47b8e-186">İkinci gönderilen form değerleri içeren HTTP POST isteği işler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="47b8e-187">Denetleyici eylemini ASP.NET MVC, yalnızca HTTP POST isteklerini yanıtlaması gerektiğini söyleyen bir [HttpPost] özniteliği olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="47b8e-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="47b8e-188">Bu eylem, dört sorumlulukları vardır:</span><span class="sxs-lookup"><span data-stu-id="47b8e-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="47b8e-189">Form değerleri okuma</span><span class="sxs-lookup"><span data-stu-id="47b8e-189">Read the form values</span></span>
- 2. <span data-ttu-id="47b8e-190">Form değerleri tüm doğrulama kuralları geçirdiğiniz denetleyin</span><span class="sxs-lookup"><span data-stu-id="47b8e-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="47b8e-191">Form gönderme geçerliyse, verileri Kaydet ve güncelleştirilmiş listesini görüntüleme</span><span class="sxs-lookup"><span data-stu-id="47b8e-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="47b8e-192">Form gönderme geçerli değilse, doğrulama hataları olan formu yeniden görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="47b8e-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="47b8e-193">Model bağlama ile okuma Form değerleri</span><span class="sxs-lookup"><span data-stu-id="47b8e-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="47b8e-194">Denetleyici eylemini başlık, fiyat ve AlbumArtUrl değerlerinden GenreId ve ArtistId (aşağı açılan listeden) ve metin değerleri içeren bir form gönderme işliyor.</span><span class="sxs-lookup"><span data-stu-id="47b8e-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="47b8e-195">Form değerleri doğrudan erişmesini mümkün olsa da, daha iyi bir yaklaşım ASP.NET MVC yerleşik Model bağlama özelliklerini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="47b8e-196">Bir denetleyici eylemi bir model türünü bir parametre olarak aldığında, ASP.NET MVC form girişleri (yanı sıra yol ve sorgu dizesi değerleri) kullanarak bu türde bir nesne doldurmak dener.</span><span class="sxs-lookup"><span data-stu-id="47b8e-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="47b8e-197">Bunu adları eşleşen model nesnesi özelliklerini örneğin değerlerini bakarak yapar yeni albüm nesnenin GenreId değeri ayarlanırken GenreId ada sahip bir giriş arar.</span><span class="sxs-lookup"><span data-stu-id="47b8e-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="47b8e-198">ASP.NET MVC uygulamasında standart yöntemlerini kullanarak görünümler oluşturduğunuzda, formları Bu alan adları yalnızca eşleşmemesidir giriş alan adları olarak özellik adlarını kullanarak her zaman işlenir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="47b8e-199">Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="47b8e-199">Validating the Model</span></span>

<span data-ttu-id="47b8e-200">Model ModelState.IsValid basit çağrısıyla doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="47b8e-201">Henüz hiçbir doğrulama kuralları bizim albüm sınıfına eklemediniz - bit - bu nedenle şu anda bu denetimi yapmak için çok yok bunu.</span><span class="sxs-lookup"><span data-stu-id="47b8e-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="47b8e-202">Önemli olan bu ModelStat.IsValid onay doğrulama kuralları ileride yapılacak değişiklikler denetleyici eylem kodu herhangi bir güncelleştirme gerektiren olmaz böylece biz bizim model üzerinde put doğrulama kuralları Uyarlanır ' dir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="47b8e-203">Gönderilen değerler kaydetme</span><span class="sxs-lookup"><span data-stu-id="47b8e-203">Saving the submitted values</span></span>

<span data-ttu-id="47b8e-204">Form gönderme doğrulama başarılı olursa, bu değerleri veritabanına kaydetmek için saattir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="47b8e-205">Entity Framework ile yalnızca model albümleri koleksiyona ekleme ve SaveChanges çağırma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="47b8e-206">Entity Framework değeri kalıcı hale getirmek için uygun SQL komutlarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="47b8e-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="47b8e-207">Bizim güncelleştirme görebiliriz böylece veriler kaydedildikten sonra biz albümleri listeye yeniden yönlendirin.</span><span class="sxs-lookup"><span data-stu-id="47b8e-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="47b8e-208">Bu, görüntülenen istiyoruz denetleyici eylemi adıyla RedirectToAction döndürerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="47b8e-209">Bu durumda, dizin yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="47b8e-210">Geçersiz form gönderimlerini doğrulama hataları ile görüntüleme</span><span class="sxs-lookup"><span data-stu-id="47b8e-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="47b8e-211">Geçersiz form girişi durumunda açılır değerleri (HTTP-GET izlemesinde) ViewBag eklenir ve ilişkili model değerler görüntülenmek üzere geri görünümüne geçirilir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="47b8e-212">Doğrulama hataları, kullanarak otomatik olarak görüntülenir @Html.ValidationMessageFor HTML Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="47b8e-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="47b8e-213">Form Oluştur test etme</span><span class="sxs-lookup"><span data-stu-id="47b8e-213">Testing the Create Form</span></span>

<span data-ttu-id="47b8e-214">Bu, çalışma /StoreManager/Oluştur / - göz atın ve uygulama test etmek için bu, StoreController oluşturma HTTP GET yöntemi tarafından döndürülen boş bir form gösterir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="47b8e-215">Bazı değerleri doldurun ve form gönderme için Oluştur düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="47b8e-216">Düzenlemelerini işleme</span><span class="sxs-lookup"><span data-stu-id="47b8e-216">Handling Edits</span></span>

<span data-ttu-id="47b8e-217">Düzen eylem çifti (HTTP-GET ve HTTP POST) yalnızca inceledik oluşturma eylem yöntemlerine çok benzer.</span><span class="sxs-lookup"><span data-stu-id="47b8e-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="47b8e-218">İçindeki rota üzerinden geçen varolan albümü, düzenleme HTTP yöntemi "id" parametresi temelinde albüm yükler GET ile çalışma düzenleme senaryo içerir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="47b8e-219">Daha önce ayrıntıları denetleyici eylemi inceledik AlbumId tarafından albüm almak için bu kodu aynıdır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="47b8e-220">Gibi oluşturma / HTTP GET yöntemi değerleri aşağı açılan ViewBag döndürülür.</span><span class="sxs-lookup"><span data-stu-id="47b8e-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="47b8e-221">Bu bize albüm bizim model nesnesi olarak (hangi albüm sınıfına kesin türü belirtilmiş) görünüme dönmek ek veriler (örneğin türler listesi) görünüm paketini geçirirken sağlar.</span><span class="sxs-lookup"><span data-stu-id="47b8e-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="47b8e-222">Düzen HTTP POST eylem oluşturma HTTP POST eyleme çok benzer.</span><span class="sxs-lookup"><span data-stu-id="47b8e-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="47b8e-223">Tek fark yeni albümü db ekleme yerine olmasıdır. Albümleri koleksiyonu db kullanarak biz albümü geçerli örneği bulma. Entry(Album) ve durumuna Modified olarak ayarlama.</span><span class="sxs-lookup"><span data-stu-id="47b8e-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="47b8e-224">Bu, yeni bir tane oluşturmak yerine var olan bir albümü değiştiriyorsunuz Entity Framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="47b8e-225">Biz bu çıkışı uygulaması çalıştıran ve tarama/StoreManger/sonra bir albümü düzenleme bağlantısını tıklatarak test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47b8e-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="47b8e-226">Bu düzen HTTP GET yöntemi tarafından gösterilen düzenleme formu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="47b8e-227">Bazı değerleri doldurun ve Kaydet düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="47b8e-228">Bu form yazılarını, değerleri kaydeder ve bize değerler güncelleştirildi gösteren albüm listeye döndürür.</span><span class="sxs-lookup"><span data-stu-id="47b8e-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="47b8e-229">İşleme silme</span><span class="sxs-lookup"><span data-stu-id="47b8e-229">Handling Deletion</span></span>

<span data-ttu-id="47b8e-230">Silme, düzenleme ve onay formu görüntülemek için bir denetleyici eylemi ve form gönderme işlemek için başka bir denetleyici eylemi kullanarak oluşturun, olarak aynı düzeni izler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="47b8e-231">HTTP GET silme denetleyici eylemi tam olarak bizim önceki Deposu Yöneticisi ayrıntıları denetleyici eylemi ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="47b8e-232">Biz kesin türü belirtilmiş bir form Delete Görünümü içerik şablonu kullanarak bir albüm türünü görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="47b8e-233">Modeli için tüm alanlar Delete şablon gösterir ancak Biz bu aşağı oldukça biraz basitleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47b8e-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="47b8e-234">/Views/StoreManager/Delete.cshtml görünüm kodda şu şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="47b8e-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="47b8e-235">Bu, Basitleştirilmiş bir silme onayı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="47b8e-236">Sil düğmesini tıklatarak DeleteConfirmed eylemi yürüten sunucuya geri, postalama forma neden olur.</span><span class="sxs-lookup"><span data-stu-id="47b8e-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="47b8e-237">Bizim HTTP POST silme denetleyici eylemi aşağıdaki eylemleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="47b8e-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="47b8e-238">Kimliğe göre albüm yükler</span><span class="sxs-lookup"><span data-stu-id="47b8e-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="47b8e-239">Albüm siler ve değişiklikleri kaydedin</span><span class="sxs-lookup"><span data-stu-id="47b8e-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="47b8e-240">Albüm listeden kaldırıldığını gösteren dizin yönlendirir</span><span class="sxs-lookup"><span data-stu-id="47b8e-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="47b8e-241">Bu test etmek için uygulamayı çalıştırın ve /StoreManager için göz atın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="47b8e-242">Albüm listeden seçip Sil bağlantısını tıklayın.</span><span class="sxs-lookup"><span data-stu-id="47b8e-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="47b8e-243">Bu bizim Sil onay ekranı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47b8e-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="47b8e-244">Sil düğmesini tıklatarak albümü kaldırır ve bize albümü silinip silinmediğini gösterir Depolama Yöneticisi dizin sayfasına döndürür.</span><span class="sxs-lookup"><span data-stu-id="47b8e-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="47b8e-245">Metin kesmek için özel bir HTML Yardımcısını kullanma</span><span class="sxs-lookup"><span data-stu-id="47b8e-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="47b8e-246">Mağaza Yöneticisi dizin sayfamızı biz olası bir sorunu açıyor.</span><span class="sxs-lookup"><span data-stu-id="47b8e-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="47b8e-247">Bizim albüm başlığı ve sanatçı adı özelliklerini hem de bunların bizim tablosunu biçimlendirme kapalı throw yeterince uzun olabilir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="47b8e-248">Bunlar ve diğer özellikleri bizim görünümlerinde kolayca kesmek için bize izin vermek için özel bir HTML Yardımcısı oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="47b8e-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="47b8e-249">Razor'ın @helper sözdizimi yaptı bu oldukça kolay kendi yardımcı işlevleri kullanmak için görünümler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="47b8e-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="47b8e-250">/Views/StoreManager/Index.cshtml görünümünü açın ve hemen sonra aşağıdaki kodu ekleyin @model satır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="47b8e-251">Bu yardımcı yöntemi, bir dize ve izin vermek için en fazla alır.</span><span class="sxs-lookup"><span data-stu-id="47b8e-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="47b8e-252">Sağlanan metin belirtilen uzunluktan daha kısaysa, yardımcı olarak çıkarır-değil.</span><span class="sxs-lookup"><span data-stu-id="47b8e-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="47b8e-253">Daha uzun olması durumunda metni keser ve işler geri kalanı "...".</span><span class="sxs-lookup"><span data-stu-id="47b8e-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="47b8e-254">Şimdi biz albüm başlığı ve sanatçı adı özelliklerini 25 karakterden az olduğundan emin olmak için bizim Truncate Yardımcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47b8e-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="47b8e-255">Aşağıda yeni bizim Truncate Yardımcısını kullanarak tam görünümü kodu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="47b8e-256">Şimdi biz /StoreManager/ URL göz attığınızda, Albümler ve başlıkları bizim en çok uzunlukları tutulur.</span><span class="sxs-lookup"><span data-stu-id="47b8e-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="47b8e-257">Not: Bu oluşturma ve tek bir görünümde bir Yardımcısını kullanarak basit bir durumda gösterir.</span><span class="sxs-lookup"><span data-stu-id="47b8e-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="47b8e-258">Siteniz kullanabilirsiniz Yardımcıları oluşturma hakkında daha fazla bilgi edinmek için my blog gönderisi bakın: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="47b8e-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="47b8e-259">[Önceki](mvc-music-store-part-4.md)
> [sonraki](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="47b8e-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
