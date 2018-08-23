---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: '10. Bölüm: Gezinti ve Site tasarımı, sonuç yönelik son güncelleştirmeler | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 10. Bölüm gezinti ve S. yönelik son güncelleştirmeler kapsayan...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f32509701dd112053aa4f31d6552601f961c7413
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757216"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="9d560-104">10. Bölüm: Gezinti ve Site tasarımı, sonuç yönelik son güncelleştirmeler</span><span class="sxs-lookup"><span data-stu-id="9d560-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="9d560-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="9d560-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="9d560-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="9d560-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="9d560-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="9d560-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="9d560-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9d560-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="9d560-109">10. Bölüm gezinti ve Site tasarımı, sonuç yönelik son güncelleştirmeler kapsar.</span><span class="sxs-lookup"><span data-stu-id="9d560-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="9d560-110">Ana işlevleri için sitemizi tamamladınız, ancak yine de site gezintisi, giriş sayfası ve Store Gözat sayfası eklemek için bazı özellikleri sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="9d560-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="9d560-111">Alışveriş sepeti Özet kısmi görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d560-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="9d560-112">Kullanıcının alışveriş sepetine öğe sayısı tüm site genelinde kullanıma sunmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="9d560-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="9d560-113">Biz kolayca bu bizim Site.master için eklenen bir kısmi görünüm oluşturarak uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d560-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="9d560-114">Daha önce gösterildiği gibi ShoppingCart denetleyicisi kısmi görünüm döndüren bir CartSummary eylem yöntemini içerir:</span><span class="sxs-lookup"><span data-stu-id="9d560-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="9d560-115">CartSummary kısmi görünümü oluşturmak için görünümler/ShoppingCart klasörü sağ tıklatın ve eklemek görünümü seçin.</span><span class="sxs-lookup"><span data-stu-id="9d560-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="9d560-116">CartSummary görünüm adını ve aşağıda gösterildiği gibi "kısmi Görünüm Oluştur" onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="9d560-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="9d560-117">Gerçekten Basit CartSummary kısmi görünüm - arabasında öğe sayısını gösteren ShoppingCart dizin görünümünün yalnızca bir bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9d560-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="9d560-118">CartSummary.cshtml için tam kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="9d560-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="9d560-119">Biz sitedeki Site asıl Html.RenderAction yöntemi kullanarak dahil olmak üzere herhangi bir sayfa kısmi görünüm ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d560-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="9d560-120">RenderAction ("CartSummary") eylem adı ve Denetleyici adı ("ShoppingCart") olarak aşağıda belirtmenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9d560-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="9d560-121">Bu düzen siteye eklemeden önce Site.master güncelleştirmelerimizin tümünü bir kerede yapabiliyoruz şekilde Tarz menü da oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="9d560-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="9d560-122">Tarz menü kısmi görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="9d560-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="9d560-123">Bu kullanıcılarımız mağazası aracılığıyla tüm türleri kullanılabilir bizim deposundaki listeleyen bir türe menü ekleyerek gitmek için çok daha kolay yapabiliyoruz.</span><span class="sxs-lookup"><span data-stu-id="9d560-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="9d560-124">Biz aynı izleyeceği adımları GenreMenu kısmi görünüm oluşturabilir ve ardından her iki Site asıl ekleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="9d560-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="9d560-125">İlk olarak, aşağıdaki GenreMenu denetleyici eylemi için StoreController ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9d560-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="9d560-126">Bu eylem, sonraki oluşturacağız kısmi görünüm tarafından görüntülenen türleri listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9d560-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="9d560-127">*Not: [ChildActionOnly] özniteliği yalnızca kısmi bir görünümden kullanılmak üzere bu eylem istediğimizi belirten Bu denetleyici eylemi ekledik. Bu öznitelik için /Store/GenreMenu göz atarak çalıştırılmasını denetleyici eylemini engeller. Bu, kısmi görünümleri için gerekli değildir ancak biz amaçladığınız gibi denetleyicisi eylemlerimiz kullanılır emin olmak istiyoruz beri iyi bir uygulama olacaktır. Biz de diğer görünümlerde eklenmiş olduğu gibi bu görünüm için Düzen kullanmamalısınız bilmeniz görünüm altyapısı sağlayan görünümü yerine PartialView iade ettiğiniz.*</span><span class="sxs-lookup"><span data-stu-id="9d560-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="9d560-128">GenreMenu denetleyici eylemi sağ tıklayın ve bu tarz görünüm veri sınıfı aşağıda gösterildiği gibi kullanarak türü kesin olarak GenreMenu adlı bir kısmi görünüm oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9d560-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="9d560-129">Sırasız bir listesini kullanarak aşağıdaki gibi öğeleri görüntülemek GenreMenu kısmi görünüm için Görünüm Kodu güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="9d560-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="9d560-130">Site, kısmi görünümleri görüntülemek için Düzen güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="9d560-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="9d560-131">Bizim kısmi görünümler Site düzenine ekleyebilirsiniz (/görünümler/paylaşılan/\_Layout.cshtml) Html.RenderAction() çağırarak.</span><span class="sxs-lookup"><span data-stu-id="9d560-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="9d560-132">İçinde ikisini yanı sıra bazı ek biçimlendirme bunları görüntülemek için aşağıda gösterildiği gibi ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="9d560-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="9d560-133">Biz uygulamayı çalıştırdığınızda, artık sol gezinti bölmesinde tarz ve üst Sepeti özeti göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d560-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="9d560-134">Store Gözat sayfaya güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="9d560-134">Update to the Store Browse page</span></span>

<span data-ttu-id="9d560-135">Store Gözat sayfanın işe yarar, ancak çok iyi görünmüyor.</span><span class="sxs-lookup"><span data-stu-id="9d560-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="9d560-136">Biz (/Views/Store/Browse.cshtml içinde bulunur) kodu görüntüle aşağıdaki gibi güncelleştirerek daha iyi bir düzende albümleri gösterecek şekilde sayfayı güncelleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9d560-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="9d560-137">Burada yapıyoruz Html.ActionLink yerine Url.Action kullanabilir, böylece biz bağlantısını albüm resim eklemek için özel bir biçimlendirme uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d560-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="9d560-138">*Not: Biz bu albümleri için bir genel albümü kapak görüntülüyorsunuz. Bu bilgiler veritabanında depolanır ve Store Yöneticisi düzenlenebilir. Kendi resim eklemek Hoş Geldiniz.*</span><span class="sxs-lookup"><span data-stu-id="9d560-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="9d560-139">Şimdi biz için bir türe göz attığınızda, albüm resimleri içeren bir kılavuz gösterilen albümleri göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d560-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="9d560-140">Üst satış albümleri göstermek için giriş sayfası güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="9d560-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="9d560-141">Satışları artırmak için giriş sayfasında albümleri satış bizim üst özellik istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="9d560-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="9d560-142">Bizim HomeController işleyen ve bazı ek grafikler, eklemek için bazı güncelleştirmeler oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="9d560-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="9d560-143">EntityFramework ilişkili bilebilmesi ilk olarak bir gezinti özelliği bizim albüm sınıfına ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d560-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="9d560-144">Son birkaç satırı bizim **albüm** sınıfı artık şu şekilde görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="9d560-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="9d560-145">*Not: Bu kullanarak bir ekleme gerekir deyimini System.Collections.Generic ad alanına getirin.*</span><span class="sxs-lookup"><span data-stu-id="9d560-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="9d560-146">İlk olarak, bir storeDB alan ve diğer bizim denetleyicileri olduğu gibi deyimlerini MvcMusicStore.Models ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d560-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="9d560-147">Ardından, veritabanımızdaki OrderDetails göre en çok satan albümleri bulmak için sorguları HomeController için aşağıdaki yöntemi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d560-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="9d560-148">Bir denetleyici eylemi olarak kullanılabilir olmasını istemiyorsanız bu yana özel bir yöntem budur.</span><span class="sxs-lookup"><span data-stu-id="9d560-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="9d560-149">Biz bunu HomeController kolaylık olması için de dahil, ancak iş mantığınızı ayrı hizmet sınıflarını uygun şekilde içine taşımak için önerilir.</span><span class="sxs-lookup"><span data-stu-id="9d560-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="9d560-150">Bu yerinde albümleri satış en çok kullanılan 5 sorgulamak ve bunları görünümüne dönmek için dizin denetleyici eylemi güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9d560-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="9d560-151">Güncelleştirilmiş HomeController için tam kod, aşağıda gösterildiği gibi ' dir.</span><span class="sxs-lookup"><span data-stu-id="9d560-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="9d560-152">Son olarak, biz Model türü güncelleştiriliyor ve albüm listenin en altına ekleyerek albümleri listesini görüntüleyebilmesi bizim giriş dizini görünümünü güncelleştirme gerekir.</span><span class="sxs-lookup"><span data-stu-id="9d560-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="9d560-153">Biz de sayfaya başlık ve bir yükseltme bölümü eklemek için bu fırsattan yararlanın.</span><span class="sxs-lookup"><span data-stu-id="9d560-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="9d560-154">Şimdi biz uygulamayı çalıştırdığınızda, güncelleştirilmiş giriş sayfamızı üst satış Albümler ve bizim promosyon ileti göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="9d560-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="9d560-155">Sonuç</span><span class="sxs-lookup"><span data-stu-id="9d560-155">Conclusion</span></span>

<span data-ttu-id="9d560-156">ASP.NET MVC kolaylaştırır olduğunu gördük veritabanı erişimi, üyelik, AJAX, Gelişmiş bir Web sitesi oluşturmak için vs.</span><span class="sxs-lookup"><span data-stu-id="9d560-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="9d560-157">oldukça hızlı bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="9d560-157">pretty quickly.</span></span> <span data-ttu-id="9d560-158">Umarım Bu öğretici, kendi ASP.NET MVC uygulamaları oluşturmaya başlamak için ihtiyacınız olan araçları verdiği!</span><span class="sxs-lookup"><span data-stu-id="9d560-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="9d560-159">Önceki</span><span class="sxs-lookup"><span data-stu-id="9d560-159">Previous</span></span>](mvc-music-store-part-9.md)
