---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: '8. Kısım: Alışveriş sepeti Ajax güncelleştirmeleriyle | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 8 Ajax güncelleştirmelerini ile alışveriş sepeti kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871291"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="bc441-104">8. Kısım: Alışveriş sepetine Ajax güncelleştirmelerle</span><span class="sxs-lookup"><span data-stu-id="bc441-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="bc441-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="bc441-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="bc441-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="bc441-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="bc441-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="bc441-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="bc441-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="bc441-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="bc441-109">Bölümü 8 Ajax güncelleştirmelerini ile alışveriş sepeti kapsar.</span><span class="sxs-lookup"><span data-stu-id="bc441-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="bc441-110">Kaydettirmeden kendi sepetinizde albümleri yerleştirmek kullanıcılara izin vermek, ancak tam checkout konuklar olarak kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bc441-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="bc441-111">Alışveriş ve kullanıma alma işlemi iki denetleyicilerinde ayrılacaktır: anonim bir sepetine öğe eklemeyi sağlayan bir ShoppingCart denetleyici ve ödeme işlemi işleyen bir Checkout denetleyici.</span><span class="sxs-lookup"><span data-stu-id="bc441-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="bc441-112">Biz bu bölümdeki alışveriş sepetine ile başlayın, ardından derleme şu bölümdeki kullanıma alma işlemi.</span><span class="sxs-lookup"><span data-stu-id="bc441-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="bc441-113">Sepeti, sipariş ve OrderDetail model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="bc441-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="bc441-114">Bizim alışveriş sepeti ve satın alma işlemleri yapar bazı yeni sınıflar kullanın.</span><span class="sxs-lookup"><span data-stu-id="bc441-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="bc441-115">Modeller klasörü sağ tıklatın ve aşağıdaki kod ile Sepeti sınıfı (Cart.cs) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc441-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="bc441-116">Bu sınıf, o ana kadarki öznitelik dışında [anahtarı] RecordID özelliği için kullandığımız başkalarına oldukça benzer.</span><span class="sxs-lookup"><span data-stu-id="bc441-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="bc441-117">Bizim Sepeti öğeleri anonim alışveriş izin vermek için CartID adlı bir dize tanımlayıcısına sahip olacaktır, ancak tablo RecordID adlı bir tamsayı birincil anahtar içerir.</span><span class="sxs-lookup"><span data-stu-id="bc441-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="bc441-118">Kurala göre Entity Framework kod-ilk Sepeti adlı bir tablo için birincil anahtar CartId veya kimliği olacaktır, ancak biz istiyorsanız, biz kolayca, ek açıklama ve kod kılabilirsiniz bekliyor.</span><span class="sxs-lookup"><span data-stu-id="bc441-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="bc441-119">Bu bir örnek biz basit kuralları Entity Framework kod-ilk nasıl kullanabileceğinizi bize uygun olduğunda, ancak sunulmuyorsa, biz onlar tarafından kısıtlı değil.</span><span class="sxs-lookup"><span data-stu-id="bc441-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="bc441-120">Ardından, sipariş sınıfı (Order.cs) ile aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc441-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="bc441-121">Bu sınıf, Sipariş Özeti ve teslimat bilgileri izler.</span><span class="sxs-lookup"><span data-stu-id="bc441-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="bc441-122">**Henüz derleme olmaz**, henüz oluşturduğumuz henüz bir sınıf üzerinde bağımlı bir sipariş ayrıntıları gezinti özelliği olduğundan.</span><span class="sxs-lookup"><span data-stu-id="bc441-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="bc441-123">Şimdi şimdi ekleyerek bir sınıf OrderDetail.cs, aşağıdaki kod ekleme adlandırdığınız düzeltin.</span><span class="sxs-lookup"><span data-stu-id="bc441-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="bc441-124">Bir son güncelleştirme ayrıca bir DbSet dahil olmak üzere bu yeni Model sınıfları kullanıma DbSets dahil etmek için bizim MusicStoreEntities sınıfı sağlayacaksınız&lt;sanatçı&gt;.</span><span class="sxs-lookup"><span data-stu-id="bc441-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="bc441-125">Güncelleştirilmiş MusicStoreEntities sınıfı olarak görünür aşağıda.</span><span class="sxs-lookup"><span data-stu-id="bc441-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="bc441-126">Alışveriş sepetine iş mantığı yönetme</span><span class="sxs-lookup"><span data-stu-id="bc441-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="bc441-127">Ardından, modelleri klasöründe ShoppingCart sınıfı oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="bc441-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="bc441-128">ShoppingCart modeli Sepeti tabloya veri erişimi işler.</span><span class="sxs-lookup"><span data-stu-id="bc441-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="bc441-129">Ayrıca, bu öğeler ekleme ve Alışveriş sepetinden kaldırma için iş mantığı işleyecek.</span><span class="sxs-lookup"><span data-stu-id="bc441-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="bc441-130">Yalnızca kendi alışveriş sepetine öğe eklemek bir hesap için kaydolun gerektirmek istemediğiniz olduğundan, biz kullanıcıların (bir GUID veya genel benzersiz tanımlayıcısını kullanarak) geçici benzersiz bir tanımlayıcı atar alışveriş sepeti eriştiklerinde.</span><span class="sxs-lookup"><span data-stu-id="bc441-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="bc441-131">ASP.NET oturum sınıfını kullanarak bu kimliği depolarız.</span><span class="sxs-lookup"><span data-stu-id="bc441-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="bc441-132">*Not: ASP.NET oturum site çıktıktan sonra sona erecek kullanıcıya özgü bilgileri depolamak için uygun bir yerdir. Oturum durumu kötüye kullanılması büyük sitelerinde performans etkileri sahip olsa da, hafif kullanma da gösterim amaçları için çalışır.*</span><span class="sxs-lookup"><span data-stu-id="bc441-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="bc441-133">ShoppingCart sınıf aşağıdaki yöntemleri sunar:</span><span class="sxs-lookup"><span data-stu-id="bc441-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="bc441-134">**AddToCart** albüm bir parametre olarak alır ve kullanıcının sepetine ekler.</span><span class="sxs-lookup"><span data-stu-id="bc441-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="bc441-135">Sepeti tablonun her albüm miktarını izler olduğundan, gerekiyorsa yeni bir satır oluşturun veya kullanıcı albümü bir kopyası zaten sipariş miktar yalnızca artırmak için mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="bc441-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="bc441-136">**RemoveFromCart** albüm kimliği alır ve kullanıcının sepetinden kaldırır.</span><span class="sxs-lookup"><span data-stu-id="bc441-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="bc441-137">Kullanıcı kendi sepetinizde yalnızca albümü bir kopyasını olsaydı, satır kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="bc441-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="bc441-138">**EmptyCart** kullanıcının Alışveriş sepetinden tüm öğeleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="bc441-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="bc441-139">**GetCartItems** görüntü veya işlem için CartItems listesini alır.</span><span class="sxs-lookup"><span data-stu-id="bc441-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="bc441-140">**GetCount** alır bir bir kullanıcı, kendi alışveriş sepeti sahip albümleri toplam sayısı.</span><span class="sxs-lookup"><span data-stu-id="bc441-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="bc441-141">**GetTotal** sepetteki tüm öğelerin toplam maliyeti hesaplar.</span><span class="sxs-lookup"><span data-stu-id="bc441-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="bc441-142">**CreateOrder** alışveriş sepeti siparişe checkout aşamasında dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="bc441-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="bc441-143">**GetCart** Sepeti nesnesi edinmek bizim denetleyicileri sağlayan statik bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="bc441-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="bc441-144">Kullandığı **GetCartId** CartId kullanıcının oturumunu okuma işlemek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="bc441-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="bc441-145">Kullanıcının oturumunu kullanıcının CartId okuyabilmeniz GetCartId yöntemi ortamdan HttpContextBase gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bc441-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="bc441-146">İşte tam **ShoppingCart sınıfı**:</span><span class="sxs-lookup"><span data-stu-id="bc441-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="bc441-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="bc441-147">ViewModels</span></span>

<span data-ttu-id="bc441-148">Bizim alışveriş sepeti denetleyicisi bizim Model nesneleri düzgün bir şekilde eşlenmez bazı karmaşık bilgileri kendi görünümlere iletişim kurmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="bc441-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="bc441-149">Bizim görünümleri uyacak şekilde bizim modelleri değiştirmek istemiyorsanız; Model sınıfları bizim etki alanı, kullanıcı arabirimi temsil etmelidir.</span><span class="sxs-lookup"><span data-stu-id="bc441-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="bc441-150">Mağaza Yöneticisi açılır bilgilerle yaptığımız, ancak çok sayıda bilgi ViewBag geçirme yönetmek sabit alır ViewBag sınıfını kullanarak bizim görünümlerine bilgi aktarmak için bir çözüm olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc441-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="bc441-151">Bu çözüme kullanmaktır *ViewModel* düzeni.</span><span class="sxs-lookup"><span data-stu-id="bc441-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="bc441-152">Bu deseni kullanılırken bizim belirli görünüm senaryolar için iyileştirilen ve hangi görünüm şablonlarımız tarafından gerekli dinamik değerler/içerik özelliklerini ortaya kesin türü belirtilmiş sınıfları oluşturuyoruz.</span><span class="sxs-lookup"><span data-stu-id="bc441-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="bc441-153">Bizim denetleyicisi sınıfları sonra doldurmak ve bu görünüm için iyileştirilmiş sınıfları kullanmak üzere bizim görünüm şablonu geçirin.</span><span class="sxs-lookup"><span data-stu-id="bc441-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="bc441-154">Bu tür güvenliği, derleme zamanı denetlemek ve düzenleyici IntelliSense görünümü şablonlar içindeki sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc441-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="bc441-155">Bizim alışveriş sepeti denetleyicisi kullanmak için iki görünüm modeli oluşturacağız: ShoppingCartViewModel kullanıcının alışveriş sepeti içeriğini tutacak ve ShoppingCartRemoveViewModel bir kullanıcı bir şey çıkardığında onay bilgilerini görüntülemek için kullanılır kendi sepetinden.</span><span class="sxs-lookup"><span data-stu-id="bc441-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="bc441-156">Yeni bir ViewModels klasör düzenli tutmak için bizim proje kök dizininde oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="bc441-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="bc441-157">Projeyi sağ tıklatın, Ekle'yi seçin / yeni bir klasör.</span><span class="sxs-lookup"><span data-stu-id="bc441-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="bc441-158">ViewModels klasörün adı.</span><span class="sxs-lookup"><span data-stu-id="bc441-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="bc441-159">Ardından, ShoppingCartViewModel sınıfı ViewModels klasöründe ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc441-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="bc441-160">İki özelliklere sahiptir: Sepeti öğeleri ve tüm öğeler için toplam fiyatı sepetinizde tutmak için ondalık bir değer listesi.</span><span class="sxs-lookup"><span data-stu-id="bc441-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="bc441-161">Şimdi ShoppingCartRemoveViewModel ViewModels klasörüyle aşağıdaki dört özellikleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc441-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="bc441-162">Alışveriş sepeti denetleyicisi</span><span class="sxs-lookup"><span data-stu-id="bc441-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="bc441-163">Alışveriş sepetine denetleyicisi üç ana amacı vardır: bir sepetine öğe ekleme, Alışveriş sepetinden öğeleri kaldırma ve sepetinizde öğeleri görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="bc441-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="bc441-164">Kullanmak üç sınıflarını biz yapacak yeni oluşturduğunuz: ShoppingCartViewModel, ShoppingCartRemoveViewModel ve ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="bc441-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="bc441-165">StoreController ve StoreManagerController olduğu gibi MusicStoreEntities örneği tutmak için bir alan ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="bc441-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="bc441-166">Yeni bir alışveriş sepeti denetleyicisi boş denetleyicisi şablonu kullanarak projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc441-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="bc441-167">Tam ShoppingCart denetleyicisi aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="bc441-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="bc441-168">Dizin ve denetleyici Ekle Eylemler tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="bc441-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="bc441-169">Kaldır ve CartSummary denetleyici eylemleri aşağıdaki bölümde ele alacağız iki özel durumları işler.</span><span class="sxs-lookup"><span data-stu-id="bc441-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="bc441-170">JQuery AJAX güncelleştirmelerle</span><span class="sxs-lookup"><span data-stu-id="bc441-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="bc441-171">Sonraki ShoppingCartViewModel kesin türü belirtilmiş ve önce yöntemi olarak kullanarak liste görünümü şablonu kullanan bir alışveriş sepeti dizin sayfası oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="bc441-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="bc441-172">Ancak, Alışveriş sepetinden öğeleri kaldırmak için bir Html.ActionLink kullanmak yerine, jQuery "yukarı RemoveLink HTML sınıfı bu görünümde tüm bağlantılar için click olayını wire için" kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="bc441-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="bc441-173">Form gönderme yerine bu click olay işleyicisi yalnızca bir AJAX geri çağırma bizim RemoveFromCart denetleyici eylemi hale getirir.</span><span class="sxs-lookup"><span data-stu-id="bc441-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="bc441-174">Bir JSON serisi haline getirilen sonuç RemoveFromCart verir bizim jQuery geri çağırma daha sonra ayrıştırır ve jQuery kullanma sayfaya dört Hızlı güncelleştirmeleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="bc441-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="bc441-175">Silinen albüm listeden kaldırır</span><span class="sxs-lookup"><span data-stu-id="bc441-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="bc441-176">Sepeti sayısı güncelleştirir</span><span class="sxs-lookup"><span data-stu-id="bc441-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="bc441-177">Kullanıcı için bir güncelleştirme iletisi görüntüler</span><span class="sxs-lookup"><span data-stu-id="bc441-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="bc441-178">Sepeti toplam fiyat güncelleştirir</span><span class="sxs-lookup"><span data-stu-id="bc441-178">Updates the cart total price</span></span>

<span data-ttu-id="bc441-179">Kaldır senaryo bir Ajax geri çağırma dizin görünümünün içinde tarafından işlenen beri RemoveFromCart eylemi için ek bir görünüm gerekmez.</span><span class="sxs-lookup"><span data-stu-id="bc441-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="bc441-180">/ShoppingCart/Index görünüm için tam kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="bc441-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="bc441-181">Bunu test etmek için şu öğeler için alışveriş sepetimizi eklemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="bc441-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="bc441-182">Biz güncelleştireceğim bizim **deposu ayrıntıları** "Sepete Ekle" düğmesini içerecek biçimde görünümü.</span><span class="sxs-lookup"><span data-stu-id="bc441-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="bc441-183">Yerinde ki olsa da, bazı ekledik albüm ek bilgiler dahil edebilirsiniz Biz bu görünümünün son güncelleştirmesinden: Tarz, sanatçı, fiyat ve albüm resmi.</span><span class="sxs-lookup"><span data-stu-id="bc441-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="bc441-184">Güncelleştirilmiş deposu Ayrıntıları görünümü kodu aşağıda gösterildiği gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="bc441-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="bc441-185">Şimdi biz mağazayla tıklayın ve test ekleme ve albümleri alışveriş sepetimizi gelen ve giden kaldırılıyor.</span><span class="sxs-lookup"><span data-stu-id="bc441-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="bc441-186">Uygulamayı çalıştırın ve depolama dizinine göz atın.</span><span class="sxs-lookup"><span data-stu-id="bc441-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="bc441-187">Ardından, Albümler listesini görüntülemek için bir tarzını'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bc441-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="bc441-188">Şimdi bir albüm başlığına tıklamak "Sepete Ekle" düğmesi de dahil olmak üzere bizim güncelleştirilmiş Albüm Ayrıntıları görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc441-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="bc441-189">"Sepete Ekle" düğmesini tıklatarak bizim alışveriş sepeti dizin görünüm alışveriş sepeti Özet listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc441-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="bc441-190">Alışveriş Sepetiniz yüklendikten sonra alışveriş Sepetiniz Ajax güncelleştirme görmek için Sepeti bağlantısından Kaldır tıklatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc441-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="bc441-191">Kaydı kullanıcıların kendi sepetine öğeleri eklemesine olanak veren alışveriş çalışma çıkışı temel aldık.</span><span class="sxs-lookup"><span data-stu-id="bc441-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="bc441-192">Aşağıdaki bölümde, bunları kaydetmek ve kullanıma alma işlemini tamamlamak izin veriyoruz.</span><span class="sxs-lookup"><span data-stu-id="bc441-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="bc441-193">[Önceki](mvc-music-store-part-7.md)
> [sonraki](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="bc441-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
