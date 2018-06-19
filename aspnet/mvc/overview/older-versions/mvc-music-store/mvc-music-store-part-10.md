---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Bölüm 10: Gezinti ve Site tasarımı, sonuç son güncelleştirmeleri | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 10 gezinti ve S. son güncelleştirmeleri kapsayan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878600"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="9be8c-104">Bölüm 10: Gezinti ve Site tasarımı, sonuç son güncelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="9be8c-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="9be8c-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="9be8c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="9be8c-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="9be8c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="9be8c-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="9be8c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="9be8c-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="9be8c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="9be8c-109">Bölüm 10 gezinti ve Site tasarımı, sonuç son güncelleştirmeleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="9be8c-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="9be8c-110">Ana işlevleri için sitemizi tamamladınız ancak biz site gezintisi, giriş sayfası ve deposu Gözat sayfası eklemek için bazı özellikler devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="9be8c-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="9be8c-111">Alışveriş sepeti Özet kısmi görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="9be8c-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="9be8c-112">Kullanıcının alışveriş sepeti öğelerin sayısı tüm site genelinde kullanıma sunmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="9be8c-113">Biz kolayca bu bizim Site.master eklenen bir kısmi görünüm oluşturarak uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="9be8c-114">Daha önce gösterildiği gibi ShoppingCart denetleyicisi kısmi görünüm döndüren bir CartSummary eylem yöntemini içerir:</span><span class="sxs-lookup"><span data-stu-id="9be8c-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="9be8c-115">CartSummary kısmi görünüm oluşturmak için görünümler/ShoppingCart klasörü sağ tıklatın ve eklemek görünümü seçin.</span><span class="sxs-lookup"><span data-stu-id="9be8c-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="9be8c-116">CartSummary görünüm adını ve aşağıda gösterildiği gibi "kısmi Görünüm Oluştur" onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="9be8c-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="9be8c-117">CartSummary kısmi görünüm gerçekten basittir - yalnızca sepetinizde öğe sayısını gösteren ShoppingCart dizin görünümünün bir bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9be8c-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="9be8c-118">CartSummary.cshtml için tam kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="9be8c-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="9be8c-119">Sitedeki Site ana Html.RenderAction yöntemini kullanarak dahil olmak üzere herhangi bir sayfayı biz kısmi görünüm dahil edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="9be8c-120">RenderAction bize ("CartSummary") eylem adı ve Denetleyici adı ("alışveriş sepeti") olarak aşağıda belirtmenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9be8c-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="9be8c-121">Bu sitenin Düzen eklemeden önce biz bizim Site.master güncelleştirmeleri tümünün aynı anda olabilmeniz Tarz menü da oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="9be8c-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="9be8c-122">Tarz menü kısmi görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="9be8c-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="9be8c-123">Biz, çok kullanıcılarımızın tüm türler kullanılabilir bizim deposundaki listeleyen bir tarzını menü ekleyerek mağazayla gitmek için kolaylaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="9be8c-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="9be8c-124">Biz aynı izleyeceği adımları GenreMenu kısmi görünüm oluşturabilir ve ardından her ikisinin de Site asıl ekleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="9be8c-125">İlk olarak, aşağıdaki GenreMenu denetleyici eylemi StoreController ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9be8c-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="9be8c-126">Bu eylem, sonraki oluşturacağız kısmi görünüm tarafından görüntülenen türler listesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="9be8c-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="9be8c-127">*Not: Yalnızca kısmi bir görünümden kullanılacak Bu eylem istediğimizi belirten Bu denetleyici eylemi için [ChildActionOnly] özniteliği ekledik. Bu öznitelik için /Store/GenreMenu göz atarak çalıştırılmasını denetleyici eylemi engeller. Bu kısmi görünümler için gerekli değildir, ancak bizim denetleyici eylemleri biz düşündüğünüz olarak kullanılan emin olmak istiyoruz beri iyi bir uygulama olur. Biz de diğer görünümlerde eklenmiş olduğu gibi bu görünüm için Düzen kullanmamanız bilmeniz görünüm altyapısı sağlar görünümü yerine PartialView iade ettiğiniz.*</span><span class="sxs-lookup"><span data-stu-id="9be8c-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="9be8c-128">GenreMenu denetleyici eylemini sağ tıklayın ve aşağıda gösterildiği gibi Tarz görünüm veri sınıfını kullanarak kesin türü belirtilmiş GenreMenu adlı kısmi bir görünümü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9be8c-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="9be8c-129">Sırasız bir listesini kullanarak öğeleri görüntülemek GenreMenu kısmi görünüm için Görünüm kodunu güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="9be8c-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="9be8c-130">Site, kısmi görünümleri görüntülemek için düzeni güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="9be8c-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="9be8c-131">Bizim kısmi görünümler Site düzenine ekleyebilirsiniz (/görünümler/paylaşılan/\_Layout.cshtml) Html.RenderAction() çağırarak.</span><span class="sxs-lookup"><span data-stu-id="9be8c-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="9be8c-132">Bunların her iki yanı sıra bunları görüntülemek için bazı ek biçimlendirme aşağıda gösterildiği gibi ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="9be8c-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="9be8c-133">Biz uygulamayı çalıştırdığınızda, artık sol gezinti bölmesinde bir tarzını ve üst Sepeti özeti göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="9be8c-134">Mağaza Gözat sayfasına güncelleştir</span><span class="sxs-lookup"><span data-stu-id="9be8c-134">Update to the Store Browse page</span></span>

<span data-ttu-id="9be8c-135">Mağaza Gözat sayfa işlevseldir, ancak çok iyi görünmüyor.</span><span class="sxs-lookup"><span data-stu-id="9be8c-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="9be8c-136">Biz (/Views/Store/Browse.cshtml içinde bulunur) görünüm kodu gibi güncelleştirerek daha iyi bir düzende albümleri gösterecek şekilde sayfayı güncelleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9be8c-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="9be8c-137">Burada yapıyoruz Html.ActionLink yerine Url.Action biz bağlantısını albüm resim eklemek için özel biçimlendirme uygulayabilmeniz için aşağıdakileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="9be8c-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="9be8c-138">*Not: Biz bu albümleri için bir genel albüm kapak görüntülüyorsunuz. Bu bilgileri veritabanında depolanır ve mağaza yöneticisi düzenlenebilir. Kendi resim eklemek Hoş Geldiniz.*</span><span class="sxs-lookup"><span data-stu-id="9be8c-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="9be8c-139">Şimdi biz bir tarzını göz attığınızda, albüm resimle kılavuzda gösterilen albümleri göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="9be8c-140">Üst satış albümleri göstermek için giriş sayfası güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="9be8c-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="9be8c-141">Satış artırmak için giriş sayfasındaki albümleri satış bizim üst özellik istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="9be8c-142">Bazı güncelleştirmeler işleyen ve bazı ek grafiklerde eklemek için bizim HomeController için vermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="9be8c-143">Böylece ilişkili EntityFramework bilir ilk olarak, bir gezinti özelliği bizim albüm sınıfına ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="9be8c-144">Son birkaç satırlık bizim **albüm** sınıfı şimdi şöyle görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="9be8c-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="9be8c-145">*Not: Bu kullanarak bir ekleme gerektirir System.Collections.Generic ad alanında getirmek için deyimi.*</span><span class="sxs-lookup"><span data-stu-id="9be8c-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="9be8c-146">İlk olarak, bir storeDB alan ve diğer bizim denetleyicileri olduğu gibi deyimleri MvcMusicStore.Models ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="9be8c-147">Ardından, üst satış albümleri Sipariş Ayrıntıları göre bulmak için Veritabanımıza sorgular HomeController için aşağıdaki yöntemi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="9be8c-148">Bir denetleyici eylemi olarak kullanılabilir olmasını istemiyorsanız bu yana özel bir yöntem budur.</span><span class="sxs-lookup"><span data-stu-id="9be8c-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="9be8c-149">Biz bunu basitleştirmek için HomeController dahil, ancak ayrı bir hizmet sınıfları uygun olarak iş mantığınızı taşınıp önerilir.</span><span class="sxs-lookup"><span data-stu-id="9be8c-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="9be8c-150">Yerinde albümleri satış üst 5 sorgulamak ve görünümüne dönmek için dizin denetleyici eylemi güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="9be8c-151">Tam güncelleştirilmiş HomeController aşağıda gösterildiği gibi kodudur.</span><span class="sxs-lookup"><span data-stu-id="9be8c-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="9be8c-152">Son olarak, biz Model türü güncelleştirme ve albüm listenin altına ekleyerek albümleri listesini görüntüleyebilmesi bizim giriş dizini görünümünü güncelleştirme gerekir.</span><span class="sxs-lookup"><span data-stu-id="9be8c-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="9be8c-153">Biz de sayfaya başlık ve bir yükseltme bölümü eklemek için bu fırsatı kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9be8c-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="9be8c-154">Şimdi biz uygulamayı çalıştırdığınızda, güncelleştirilmiş giriş sayfamızı üst satış Albümler ve bizim promosyon iletisi göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="9be8c-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="9be8c-155">Sonuç</span><span class="sxs-lookup"><span data-stu-id="9be8c-155">Conclusion</span></span>

<span data-ttu-id="9be8c-156">ASP.NET MVC kolaylaştırır olmadığını gördük veritabanı erişimiyle, üyelik, AJAX, Gelişmiş bir Web sitesi oluşturmak için vb.</span><span class="sxs-lookup"><span data-stu-id="9be8c-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="9be8c-157">oldukça hızlı bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="9be8c-157">pretty quickly.</span></span> <span data-ttu-id="9be8c-158">Umarız Bu öğreticiyi kendi ASP.NET MVC uygulamaları oluşturmaya başlamak için ihtiyacınız olan araçları verdiği!</span><span class="sxs-lookup"><span data-stu-id="9be8c-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="9be8c-159">Önceki</span><span class="sxs-lookup"><span data-stu-id="9be8c-159">Previous</span></span>](mvc-music-store-part-9.md)
