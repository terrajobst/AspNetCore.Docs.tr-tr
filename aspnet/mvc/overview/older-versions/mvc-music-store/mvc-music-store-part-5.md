---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: '5. Bölüm: Formları düzenleme ve şablon oluşturma | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 5. Bölüm formları düzenleme ve şablon oluşturma kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: f799c4d492e88f3edcf3800e66e0a1bae3845ba2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395207"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="fc91d-104">5. Bölüm: Formları düzenleme ve şablon oluşturma</span><span class="sxs-lookup"><span data-stu-id="fc91d-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="fc91d-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="fc91d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="fc91d-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="fc91d-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="fc91d-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="fc91d-109">5. Bölüm formları düzenleme ve şablon oluşturma kapsar.</span><span class="sxs-lookup"><span data-stu-id="fc91d-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="fc91d-110">Son bölümde, biz bizim veritabanından veri yükleme ve görüntülemeden.</span><span class="sxs-lookup"><span data-stu-id="fc91d-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="fc91d-111">Bu bölümde, biz de verileri düzenleme tıklatmalarını sağlarsınız.</span><span class="sxs-lookup"><span data-stu-id="fc91d-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="fc91d-112">StoreManagerController oluşturma</span><span class="sxs-lookup"><span data-stu-id="fc91d-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="fc91d-113">Biz adlı yeni bir denetleyici oluşturarak başlarsınız **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="fc91d-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="fc91d-114">Bu denetleyici için biz ASP.NET MVC 3 araçları güncelleştirme kullanılabilir olan yapı İskelesi özelliklerinin avantajlarından yararlanmakla.</span><span class="sxs-lookup"><span data-stu-id="fc91d-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="fc91d-115">Denetleyici Ekle iletişim kutusu seçenekleri aşağıda gösterildiği gibi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="fc91d-116">Ekle düğmesine tıkladığınızda, ASP.NET MVC 3 yapı iskelesi mekanizması iyi bir iş miktarı sizin için halleder olduğunu görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="fc91d-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="fc91d-117">Yerel bir Entity Framework değişkenle yeni StoreManagerController oluşturur</span><span class="sxs-lookup"><span data-stu-id="fc91d-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="fc91d-118">Projenin görünümleri klasörüne StoreManager klasör ekler</span><span class="sxs-lookup"><span data-stu-id="fc91d-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="fc91d-119">Albüm sınıfı için kesin Create.cshtml, Delete.cshtml Details.cshtml Edit.cshtml ve Index.cshtml görünümü ekler</span><span class="sxs-lookup"><span data-stu-id="fc91d-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="fc91d-120">Yeni StoreManager denetleyici sınıfı içeren CRUD (oluşturma, okuma, güncelleştirme ve silme) nasıl ile albümü çalışılacağını bildiğinizi denetleyici eylemleri model sınıfı ve veritabanı erişimi için Entity Framework bağlamına bizim kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="fc91d-121">İskele kurulmuş bir görünümünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="fc91d-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="fc91d-122">Bizim için bu kodu oluşturuldu, ancak sadece biz Bu öğretici boyunca yazma gibi standart ASP.NET MVC kodu olduğunu unutmamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="fc91d-123">Ortak denetleyicisi kod yazma ve kesin türü belirtilmiş görünümler el ile oluşturmak harcadığı zamanı kaydetmek hedeflenen, ancak bu oluşturulan kod içinde nasıl değiştiğini gerekmez hakkında yorumlar doğrudan uyarılarla başında görülen tür değil kodu.</span><span class="sxs-lookup"><span data-stu-id="fc91d-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="fc91d-124">Bu, kodunuzu ve değiştirmek için beklenen.</span><span class="sxs-lookup"><span data-stu-id="fc91d-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="fc91d-125">Bu nedenle, hızlı düzenleme StoreManager dizin görünümüne başlayalım (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="fc91d-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="fc91d-126">Bu görünümü, bizim deposundaki albümleri düzenleme ile listeleyen bir tablo görüntüler / Ayrıntılar / Sil bağlantılarını ve albüm genel özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="fc91d-127">Bu görünümde çok yararlı olduğu AlbumArtUrl alanın kaldıracağız.</span><span class="sxs-lookup"><span data-stu-id="fc91d-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="fc91d-128">İçinde &lt;tablo&gt; bölümü görünümü kod kaldırmak &lt;th&gt; ve &lt;td&gt; AlbumArtUrl başvuruları, aşağıda vurgulanan satırları tarafından belirtildiği gibi çevreleyen öğeleri:</span><span class="sxs-lookup"><span data-stu-id="fc91d-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="fc91d-129">Değiştirilen görünümü kod şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="fc91d-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="fc91d-130">İlk göz Store Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="fc91d-130">A first look at the Store Manager</span></span>

<span data-ttu-id="fc91d-131">Artık uygulamayı çalıştırabilir ve/StoreManager/dizinine göz atın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="fc91d-132">Bu, biz yalnızca, depolama, Ayrıntılar, düzenleme ve silme için bağlantılarla birlikte albümleri listesini gösteren değiştiren Store Yöneticisi dizini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="fc91d-133">Düzenleme bağlantısını tıklatarak tarz ve sanatçının için açılır menüleri kullanarak da dahil olmak üzere albümü, bir düzenleme formunda alanları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="fc91d-134">Altındaki "Geri listesine" bağlantısına tıklayın ve ardından Albüm Ayrıntıları bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="fc91d-135">Bu, tek tek albüm için ayrıntılı bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="fc91d-136">Yeniden liste bağlantısını için Geri'yi tıklatın ve ardından bir Delete bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="fc91d-137">Bu, albüm ayrıntılarını gösteren ve biz silmek istediğinizden emin olup olmadığınızı onay iletişim kutusunda görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="fc91d-138">Alttaki Sil düğmesine tıklanarak albümü silin ve Silinen albümü gösteren dizin sayfasına dönün.</span><span class="sxs-lookup"><span data-stu-id="fc91d-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="fc91d-139">Store yöneticisiyle tamamlanmadı ancak denetleyici ve Görünüm Kodu başlatmak CRUD işlemleri için çalışma sahibiz.</span><span class="sxs-lookup"><span data-stu-id="fc91d-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="fc91d-140">Store Yöneticisi denetleyicisi koda baktığınızda</span><span class="sxs-lookup"><span data-stu-id="fc91d-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="fc91d-141">Store Yöneticisi denetleyicisi iyi miktarda kod içerir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="fc91d-142">Bu üstten alta Bahsedelim.</span><span class="sxs-lookup"><span data-stu-id="fc91d-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="fc91d-143">Bazı standart ad alanları bizim modelleri ad alanı başvuru yanı sıra, bir MVC denetleyicisi için denetleyici içerir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="fc91d-144">Denetleyici MusicStoreEntities her denetleyici eylemleri tarafından kullanılan veri erişimi için özel bir örneği vardır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="fc91d-145">Dizin Yöneticisi ve ayrıntıları Eylemler Store</span><span class="sxs-lookup"><span data-stu-id="fc91d-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="fc91d-146">Dizin görünümünün albümleri, biz Store Gözat yöntem üzerinde çalışırken, daha önce bahsettiğim gibi her albüm başvurulan tarz ve sanatçı bilgiler dahil olmak üzere bir listesini alır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="fc91d-147">Denetleyici, verimli ve bu bilgileri özgün istek için sorgulama için her albüm Tarz hem de sanatçının adını görüntüleyebilmesi dizin görünümünün bağlı nesnelere başvuru kullanılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="fc91d-148">Tam olarak aynı yazdığımız daha önce - albümü için sorgular Store denetleyicisi Ayrıntılar eylemi StoreManager denetleyicinin ayrıntıları denetleyici eylemi çalışır Find() yöntemi kullanarak ve Kimliğe göre ardından görünümü verir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="fc91d-149">Eylem yöntemleri oluşturun</span><span class="sxs-lookup"><span data-stu-id="fc91d-149">The Create Action Methods</span></span>

<span data-ttu-id="fc91d-150">Bunlar form girişi işlediğinden Oluştur eylemi şu ana kadar gördük olanları biraz farklı yöntemlerdir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="fc91d-151">Bir kullanıcı ilk /StoreManager/oluşturma/ziyaret ettiğinde boş bir form gösterilir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="fc91d-152">Bu HTML sayfası içerecek bir &lt;form&gt; açılan menüyü ve metin kutusu içeren öğeleri nerede bunlar girebilirsiniz albüm ayrıntıları girin.</span><span class="sxs-lookup"><span data-stu-id="fc91d-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="fc91d-153">Kullanıcı albüm form değerlerine dolgular, bunlar bu değişiklikleri uygulamamız veritabanı içinde kaydetmeyi geri göndermek için "Kaydet" düğmesine basabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fc91d-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="fc91d-154">Kullanıcı, "Kaydet" düğmesine bastığında &lt;form&gt; dön /StoreManager/oluşturma/URL bir HTTP POST gerçekleştirin ve gönderme &lt;form&gt; değerleri HTTP POST bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="fc91d-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="fc91d-155">ASP.NET MVC kolayca bizim StoreManagerController sınıf içindeki – /StoreManager/Create ilk HTTP GET Gözat işlemek için iki ayrı "Oluştur" eylem yöntemleri uygulamak bize etkinleştirerek bu iki URL çağırma senaryolar mantığı oluşturma bölme sağlıyor / URL ve diğer gönderilen değişiklikler, HTTP POST işlemek için.</span><span class="sxs-lookup"><span data-stu-id="fc91d-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="fc91d-156">Görünüm paketi kullanarak bir görünüme bilgi geçirme</span><span class="sxs-lookup"><span data-stu-id="fc91d-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="fc91d-157">Biz ViewBag, bu öğreticide daha önce kullandınız, ancak bunu hakkında pek fazla açıklandı henüz.</span><span class="sxs-lookup"><span data-stu-id="fc91d-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="fc91d-158">Görünüm paketi bilgileri kesin olarak belirlenmiş model nesnesi kullanmadan görünüme iletilecek sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="fc91d-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="fc91d-159">Bu durumda, HTTP GET Düzenle denetleyicisi eylemimiz açılır menüleri kullanarak doldurmak için forma bir listesi türleri ve sanatçıların geçirmek gereken ve bunu yapmanın en kolay yolu ViewBag öğeleri olarak döndürülecek olan.</span><span class="sxs-lookup"><span data-stu-id="fc91d-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="fc91d-160">Görünüm paketini ViewBag.Foo veya ViewBag.YourNameHere bu özelliklerini tanımlamak için kod yazmaya gerek kalmadan yazabilirsiniz, yani dinamik bir nesne olan.</span><span class="sxs-lookup"><span data-stu-id="fc91d-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="fc91d-161">Bu durumda, bunlar ayarı Albüm özellikleri GenreId ve ArtistId, formun ile gönderilen açılır değerleri olması denetleyici kodlarının ViewBag.GenreId ve ViewBag.ArtistId kullanır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="fc91d-162">Bu açılır değerleri yalnızca bu amaç için oluşturulan SelectList nesnesini kullanarak forma döndürülür.</span><span class="sxs-lookup"><span data-stu-id="fc91d-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="fc91d-163">Bunu yapmanız bu kodu kullanarak:</span><span class="sxs-lookup"><span data-stu-id="fc91d-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="fc91d-164">Eylem yöntemi koddan görebileceğiniz gibi bu nesnenin oluşturulacağı üç parametreler kullanılıyor:</span><span class="sxs-lookup"><span data-stu-id="fc91d-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="fc91d-165">Açılan listeyi görüntüleme öğeleri listesi.</span><span class="sxs-lookup"><span data-stu-id="fc91d-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="fc91d-166">Bu yalnızca bir dize değil - biz türleri listesini geçirme unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="fc91d-167">SelectList için geçirilen sonraki parametre seçtiğiniz bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="fc91d-168">Bu nasıl SelectList önceden listesindeki bir öğeyi seçmek nasıl bilir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="fc91d-169">Bu, oldukça benzer düzenleme formunda baktığımızda anlamak daha kolay olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="fc91d-170">Görüntülenecek özelliği son parametredir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="fc91d-171">Genre.Name özelliği kullanıcıya gösterilecek olduğunu gösteren bu durumda, bu.</span><span class="sxs-lookup"><span data-stu-id="fc91d-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="fc91d-172">Böylece aklınızda daha sonra HTTP GET Oluştur eylemi oldukça basittir - iki SelectLists ViewBag için eklenir ve model nesnesi yok (Bu henüz oluşturulmadıysa bu yana) biçimine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="fc91d-173">HTML Yardımcıları oluşturma Görünümü'nde açılan listeleri görüntülemek için</span><span class="sxs-lookup"><span data-stu-id="fc91d-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="fc91d-174">Değerleri açılan görünümüne geçirilir nasıl hakkında konuştuk olduğundan, bu değerlerin gösterilme biçimini görmek için görüntüyü hızlı bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="fc91d-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="fc91d-175">Görünüm kod (/ Views/StoreManager/Create.cshtml), türe açılan görüntülemek için aşağıdaki Çağrının yapıldığı görürsünüz aşağı.</span><span class="sxs-lookup"><span data-stu-id="fc91d-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="fc91d-176">Bu, bir HTML Yardımcısı - genel bir görünüm görev gerçekleştiren bir yardımcı yöntem bilinir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="fc91d-177">HTML Yardımcıları görünümü kodumuz kısa süren ve okunabilir sağlamadaki çok yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="fc91d-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="fc91d-178">Html.DropDownList Yardımcısı, ASP.NET MVC tarafından sağlanır, ancak daha sonra anlatıldığı gibi kodu görüntüle biz uygulamamız yeniden için kendi Yardımcıları oluşturmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="fc91d-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="fc91d-179">Html.DropDownList çağrı yalnızca iki şey - get görüntülemek için listede ve (varsa) hangi değeri önceden seçilmiş olmalıdır nerede olduğunu gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="fc91d-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="fc91d-180">İlk parametre, GenreId, modeli veya ViewBag GenreId adlı değeri aramak için DropDownList söyler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="fc91d-181">İkinci parametre olarak ilk açılan listeden seçilen gösterilecek değeri belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="fc91d-182">Bu form, form oluştur olduğundan, hiçbir değer seçilmiş olması ve String.Empty geçirilir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="fc91d-183">Gönderilen Form değerleri işleme</span><span class="sxs-lookup"><span data-stu-id="fc91d-183">Handling the Posted Form values</span></span>

<span data-ttu-id="fc91d-184">Önce Bahsettiğimiz gibi her bir formla ilişkili iki eylem yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="fc91d-185">İlk HTTP GET isteği işler ve form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="fc91d-186">İkinci gönderilen form değerleri içeren HTTP POST isteği işler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="fc91d-187">Denetleyici eylemini ASP.NET MVC, yalnızca HTTP POST isteklerini yanıtlaması gerektiğini söyleyen bir [HttpPost] özniteliği olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="fc91d-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="fc91d-188">Bu eylem, dört sorumluluklara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="fc91d-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="fc91d-189">Form değerlerini okuma</span><span class="sxs-lookup"><span data-stu-id="fc91d-189">Read the form values</span></span>
- 2. <span data-ttu-id="fc91d-190">Form değerlerini geçirdiğiniz tüm doğrulama kurallarını denetleyin</span><span class="sxs-lookup"><span data-stu-id="fc91d-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="fc91d-191">Form gönderme geçerliyse, verileri kaydetmek ve güncelleştirilmiş listesini görüntüleme</span><span class="sxs-lookup"><span data-stu-id="fc91d-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="fc91d-192">Form gönderme geçerli değilse, doğrulama hata formu görüntülemek</span><span class="sxs-lookup"><span data-stu-id="fc91d-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="fc91d-193">Okuma Form değerleri Model bağlama</span><span class="sxs-lookup"><span data-stu-id="fc91d-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="fc91d-194">Denetleyici eylemini başlık, fiyat ve AlbumArtUrl değerlerinden GenreId ve ArtistId (aşağı açılan listeden) ve metin değerlerini içeren bir form gönderimi işliyor.</span><span class="sxs-lookup"><span data-stu-id="fc91d-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="fc91d-195">Form değerlerini doğrudan erişmek mümkün olsa da, daha iyi bir yaklaşım ASP.NET MVC yerleşik bağlama modeli özelliklerini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="fc91d-196">Bir denetleyici eylemi bir model türünü bir parametre olarak alan, ASP.NET MVC form girişleri (hem de Yönlendirme ve sorgu dizesi değerleri) kullanarak bu türde bir nesne doldurun dener.</span><span class="sxs-lookup"><span data-stu-id="fc91d-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="fc91d-197">Bunu adları eşleşen özellikler model nesnesi, örn değerlerini bakarak yapar yeni albüm nesnenin GenreId değerini ayarlarken GenreId ada sahip bir girdi arar.</span><span class="sxs-lookup"><span data-stu-id="fc91d-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="fc91d-198">Standart yöntemlerini kullanarak ASP.NET MVC görünümleri oluşturduğunuzda, formlar Bu alan adları yalnızca eşleşmemesidir girdi alanı adları, özellik adları kullanarak her zaman işlenir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="fc91d-199">Model doğrulama</span><span class="sxs-lookup"><span data-stu-id="fc91d-199">Validating the Model</span></span>

<span data-ttu-id="fc91d-200">Modelin basit ModelState.IsValid çağrısı ile doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="fc91d-201">Henüz herhangi bir doğrulama kuralları bizim albüm sınıfına eklemediniz - biz biraz - İşte şimdi bu denetimi yapmak için çok olmayan gerçekleştirirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fc91d-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="fc91d-202">Önemli olan bu ModelStat.IsValid denetimi, doğrulama kuralları ileride yapılacak değişiklikler denetleyici eylem kodu herhangi bir güncelleştirme yapılması gerekmez, böylece biz modelimizi üzerinde yerleştirin doğrulama kurallarına göre uyarlayacak olduğu.</span><span class="sxs-lookup"><span data-stu-id="fc91d-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="fc91d-203">Gönderilen değerler kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="fc91d-203">Saving the submitted values</span></span>

<span data-ttu-id="fc91d-204">Form gönderme doğrulama testlerini geçerse, bu değerleri veritabanına kaydetme zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="fc91d-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="fc91d-205">Entity Framework ile yalnızca model albümleri koleksiyona ekleme ve SaveChanges çağırma gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="fc91d-206">Entity Framework değeri kalıcı hale getirmek için uygun SQL komutlarını oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fc91d-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="fc91d-207">Güncelleştirmemiz görebiliriz verileri kaydettikten sonra size geri albümleri listesine yönlendirmelidir, böylece.</span><span class="sxs-lookup"><span data-stu-id="fc91d-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="fc91d-208">Bu, görüntülenen istiyoruz denetleyici eylemi adıyla RedirectToAction döndürerek gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="fc91d-209">Bu durumda, dizin yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="fc91d-210">Geçersiz form gönderilerini görselleştirip doğrulama hataları ile görüntüleme</span><span class="sxs-lookup"><span data-stu-id="fc91d-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="fc91d-211">Geçersiz bir form girişi, söz konusu olduğunda açılır değerleri Görünüm (olduğu gibi HTTP GET) paketi eklenir ve bağlanmış modeli değerleri görünüme dönmek için görünen geçirilir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="fc91d-212">Doğrulama hataları, kullanarak otomatik olarak görüntülenir @Html.ValidationMessageFor HTML Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="fc91d-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="fc91d-213">Test oluşturma formu</span><span class="sxs-lookup"><span data-stu-id="fc91d-213">Testing the Create Form</span></span>

<span data-ttu-id="fc91d-214">Bu, çalışma /StoreManager/oluşturma / - göz atın ve uygulamayı test etmek için bu, StoreController oluşturma HTTP GET yöntemi tarafından döndürülen boş form gösterir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="fc91d-215">Bazı değerleri doldurun ve formunun Oluştur düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="fc91d-216">Düzenlemelerini işleme</span><span class="sxs-lookup"><span data-stu-id="fc91d-216">Handling Edits</span></span>

<span data-ttu-id="fc91d-217">Düzenleme eylem çifti (HTTP GET ve HTTP POST) yalnızca inceledik oluşturma eylem yöntemlerine oldukça benzerdir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="fc91d-218">İçinde bir rota üzerinden geçen mevcut albümü, Düzen HTTP-albümü "id" parametresini temel alan metodunun GET ile çalışma düzenleme senaryo içerir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="fc91d-219">Daha önce ayrıntı denetleyici eylemi inceledik AlbumId tarafından albüm almak için bu kodu aynı olur.</span><span class="sxs-lookup"><span data-stu-id="fc91d-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="fc91d-220">Gibi oluşturma / HTTP GET yöntemi, değerleri açılan ViewBag döndürülür.</span><span class="sxs-lookup"><span data-stu-id="fc91d-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="fc91d-221">Bu bize albüm bizim model nesnesi (hangi albüm sınıfı için kesin) görüntülemek için döndürülecek ek veriler (örneğin türleri listesi) görünüm paketini geçirirken sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc91d-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="fc91d-222">HTTP POST Düzenle eylemi için HTTP POST oluşturma eylemini çok benzer.</span><span class="sxs-lookup"><span data-stu-id="fc91d-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="fc91d-223">Tek fark DB'ye yeni albümü eklemek yerine olmasıdır. Albümleri koleksiyon db kullanarak biz albümü'nün geçerli örneğini bulma. Entry(Album) ve durumuna Modified olarak ayarlama.</span><span class="sxs-lookup"><span data-stu-id="fc91d-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="fc91d-224">Bu, yeni bir tane oluşturmak yerine var olan bir albümü değiştiriyorsunuz Entity Framework bildirir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="fc91d-225">Bu uygulamayı çalıştırma ve tarama/StoreManger/sonra albüm düzenleme bağlantısını tıklatarak sınayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="fc91d-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="fc91d-226">Bu düzen HTTP GET yöntemi tarafından gösterilen düzenleme formu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="fc91d-227">Bazı değerleri doldurun ve Kaydet düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="fc91d-228">Bu formu gönderir, değerleri kaydeder ve bize güncelleştirilmiş değerleri olduğunu gösteren albüm listesine döndürür.</span><span class="sxs-lookup"><span data-stu-id="fc91d-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="fc91d-229">İşleme silme</span><span class="sxs-lookup"><span data-stu-id="fc91d-229">Handling Deletion</span></span>

<span data-ttu-id="fc91d-230">Silme, düzenleme ve oluşturma, form gönderme işlemek için başka bir denetleyici eylemi ile onay formu görüntülemek için bir denetleyici eylemi olarak aynı deseni izler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="fc91d-231">HTTP GET Sil denetleyici eylemi tam olarak önceki Store Manager ayrıntıları denetleyicisi eylemimiz ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="fc91d-232">Delete Görünümü içerik şablonu kullanarak bir albüm türü kesin belirlenmiş bir form görüntüleriz.</span><span class="sxs-lookup"><span data-stu-id="fc91d-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="fc91d-233">Delete şablon modeli için tüm alanları gösterir, ancak biz bir bit alt basitleştirin.</span><span class="sxs-lookup"><span data-stu-id="fc91d-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="fc91d-234">/Views/StoreManager/Delete.cshtml görünümü kod şu şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="fc91d-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="fc91d-235">Bu basitleştirilmiş bir silme onayı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="fc91d-236">Sil düğmesine tıklanarak DeleteConfirmed eylemi yürüten sunucuya geri, yayımlanacak formun neden olur.</span><span class="sxs-lookup"><span data-stu-id="fc91d-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="fc91d-237">HTTP POST Sil denetleyicisi Eylemimiz aşağıdaki işlemleri yapar:</span><span class="sxs-lookup"><span data-stu-id="fc91d-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="fc91d-238">Albüm kimliği tarafından yükler</span><span class="sxs-lookup"><span data-stu-id="fc91d-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="fc91d-239">Albüm siler ve değişiklikleri kaydedin</span><span class="sxs-lookup"><span data-stu-id="fc91d-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="fc91d-240">Albüm listeden kaldırıldığını gösteren bir dizini yeniden yönlendirir</span><span class="sxs-lookup"><span data-stu-id="fc91d-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="fc91d-241">Bunu test etmek için uygulamayı çalıştırın ve /StoreManager için göz atın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="fc91d-242">Albüm listeden seçin ve Sil bağlantısını tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fc91d-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="fc91d-243">Bu, bizim silme onay ekranında görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="fc91d-244">Sil düğmesine tıklanarak albümü kaldırır ve bize albümü silindiğini gösterir Store Yöneticisi dizin sayfasına döndürür.</span><span class="sxs-lookup"><span data-stu-id="fc91d-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="fc91d-245">Metin kesmek için özel bir HTML Yardımcısını kullanma</span><span class="sxs-lookup"><span data-stu-id="fc91d-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="fc91d-246">Bir olası sorun Store Yöneticisi dizini sayfamızı yapılandırdığımıza göre.</span><span class="sxs-lookup"><span data-stu-id="fc91d-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="fc91d-247">Bizim albüm başlığı ve sanatçı adı özelliklerinin her ikisi de bunlar bizim tablo biçimlendirmeyi devre dışı durum oluşturabilir yeterince uzun olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="fc91d-248">Bize bu ve diğer özellikler bizim görünümlerde bir kolayca kesecek şekilde izin vermek için özel bir HTML Yardımcısı oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="fc91d-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="fc91d-249">Razor'ın @helper söz dizimi vermiştir, oldukça kolay kendi kullanımı için yardımcı işlevleri, görünümler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fc91d-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="fc91d-250">/Views/StoreManager/Index.cshtml görünümü açın ve hemen sonra aşağıdaki kodu ekleyin @model satır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="fc91d-251">Bu yardımcı yöntemi, bir dize ve izin vermek için bir maksimum uzunluğunu alır.</span><span class="sxs-lookup"><span data-stu-id="fc91d-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="fc91d-252">Sağlanan metnin belirtilen uzunluktan daha kısaysa yardımcı olarak çıkarır-olduğu.</span><span class="sxs-lookup"><span data-stu-id="fc91d-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="fc91d-253">Uzunsa, metni keser ve geri kalanı için "..." işler.</span><span class="sxs-lookup"><span data-stu-id="fc91d-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="fc91d-254">Bizim Truncate Yardımcısı albüm başlığı ve sanatçı adı özellikleri 25 karakterden az olduğundan emin olmak için şimdi kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="fc91d-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="fc91d-255">Yeni sunduğumuz Truncate Yardımcısı kullanarak tam görünümü kodu altında görünür.</span><span class="sxs-lookup"><span data-stu-id="fc91d-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="fc91d-256">Şimdi biz /StoreManager/ URL göz attığınızda, Albümler ve başlıkları müşterilerimizin en çok uzunlukları tutulur.</span><span class="sxs-lookup"><span data-stu-id="fc91d-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="fc91d-257">Not: Bu oluşturma ve tek bir görünümde bir Yardımcısını kullanarak basit bir durumda gösterir.</span><span class="sxs-lookup"><span data-stu-id="fc91d-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="fc91d-258">Siteniz kullanabileceğiniz Yardımcıları oluşturma hakkında daha fazla bilgi için Bilgisayarım blog gönderisi bakın: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="fc91d-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="fc91d-259">[Önceki](mvc-music-store-part-4.md)
> [İleri](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="fc91d-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
