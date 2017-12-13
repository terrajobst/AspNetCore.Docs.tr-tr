---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "9. Kısım: Kayıt ve Checkout | Microsoft Docs"
author: jongalloway
description: "Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 9 kayıt ve Checkout kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 799985f889b1063c53df7bce365ae3989809ba79
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="c32f7-104">9. Kısım: Kayıt ve kullanıma alma</span><span class="sxs-lookup"><span data-stu-id="c32f7-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="c32f7-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c32f7-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c32f7-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="c32f7-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c32f7-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c32f7-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c32f7-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c32f7-109">Bölümü 9 kayıt ve Checkout kapsar.</span><span class="sxs-lookup"><span data-stu-id="c32f7-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="c32f7-110">Bu bölümde, biz alışveriş yapanların adresini ve ödeme bilgilerini toplayan bir CheckoutController oluşturmaya olursunuz.</span><span class="sxs-lookup"><span data-stu-id="c32f7-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="c32f7-111">Biz bu denetleyici yetkilendirme gerektirecek şekilde kullanıma, önce sitemizi ile kaydetmek kullanıcıların gerektirir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="c32f7-112">Kullanıcılar için kullanıma alma işlemi, kendi Alışveriş sepetinden "Checkout" düğmesine tıklayarak gidin.</span><span class="sxs-lookup"><span data-stu-id="c32f7-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="c32f7-113">Kullanıcı günlüğe kaydedilmezse için istenir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="c32f7-114">Oturum açma başarılı olduğunda, kullanıcı ardından adresinizi ve ödeme görünümü gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="c32f7-115">Formun doldurulmuş varsa ve sırasını gönderilen sonra sipariş onay ekranında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="c32f7-116">Mevcut olmayan sipariş veya size ait olmayan bir sırada görüntüleme girişiminde hata görünümü gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="c32f7-117">Alışveriş sepeti geçirme</span><span class="sxs-lookup"><span data-stu-id="c32f7-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="c32f7-118">Kullanıcı Checkout düğmesine tıkladığında bir alışveriş işlemi anonim, olsa da, bunlar kaydetmeniz gerekir ve oturum açma.</span><span class="sxs-lookup"><span data-stu-id="c32f7-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="c32f7-119">Kullanıcıların kendi alışveriş sepeti bilgilerini ziyaretleri kayıt veya oturum açma tamamladığınızda alışveriş sepeti bilgilerin bir kullanıcıyla ilişkilendirmek ihtiyacımız şekilde korur beklediğiniz.</span><span class="sxs-lookup"><span data-stu-id="c32f7-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="c32f7-120">Bizim ShoppingCart sınıfı, geçerli sepetteki tüm öğeleri bir kullanıcı adı ile ilişkilendireceğiniz bir yöntem taşıdığından Bunu yapmak gerçekten çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="c32f7-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="c32f7-121">Biz yalnızca bir kullanıcı kaydı veya oturum açma işlemi tamamlandığında bu yöntemi çağırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="c32f7-122">Açık **AccountController** biz üyeliği ve yetkilendirme ayarlama yükleyen eklediğimiz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c32f7-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="c32f7-123">Ekleme bir MvcMusicStore.Models, başvuran deyimiyle sonra aşağıdaki MigrateShoppingCart yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c32f7-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="c32f7-124">Ardından, kullanıcı doğrulandıktan sonra aşağıda gösterildiği gibi MigrateShoppingCart çağırmak için oturum açma sonrası eylemi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c32f7-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="c32f7-125">Kullanıcı hesabı başarıyla oluşturulduktan hemen sonra eylem sonrası aynı kasaya değişiklik:</span><span class="sxs-lookup"><span data-stu-id="c32f7-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="c32f7-126">Anonim bir alışveriş sepeti başarılı kaydı veya oturum açma sırasında bir kullanıcı hesabına otomatik olarak aktarılacaktır şimdi onu - olur.</span><span class="sxs-lookup"><span data-stu-id="c32f7-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="c32f7-127">CheckoutController oluşturma</span><span class="sxs-lookup"><span data-stu-id="c32f7-127">Creating the CheckoutController</span></span>

<span data-ttu-id="c32f7-128">Denetleyicileri klasörü sağ tıklatın ve yeni bir denetleyici boş denetleyicisi şablonu kullanarak CheckoutController adlı projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c32f7-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="c32f7-129">İlk olarak, Authorize öznitelik checkout önce kaydetmek kullanıcıların denetleyicisi sınıf bildiriminin üstüne ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c32f7-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="c32f7-130">*Not: Bu biz StoreManagerController daha önce yaptığınız değişiklikleri benzer, ancak bu durumda kullanıcının yönetici rolünde olması Authorize özniteliği gerekli. Checkout denetleyicisi, kullanıcı oturum açmış olmanız biz gerektiren ancak yöneticileri olmaları gerektiren değil.*</span><span class="sxs-lookup"><span data-stu-id="c32f7-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="c32f7-131">Basitleştirmek amacıyla, biz Bu öğreticide ödeme bilgilerle ilgilenen olmaz.</span><span class="sxs-lookup"><span data-stu-id="c32f7-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="c32f7-132">Bunun yerine, biz kullanıcıların bir promosyon kodu kullanarak kullanıma izin vermiş olursunuz.</span><span class="sxs-lookup"><span data-stu-id="c32f7-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="c32f7-133">Bu promosyon kodu PromoCode adlı bir sabit kullanarak depolarız.</span><span class="sxs-lookup"><span data-stu-id="c32f7-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="c32f7-134">StoreController olduğu gibi biz storeDB adlı MusicStoreEntities sınıfının bir örneği tutmak için bir alan belirtmesi.</span><span class="sxs-lookup"><span data-stu-id="c32f7-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="c32f7-135">Yapmak için MusicStoreEntities sınıfını kullanmak, biz kullanarak bir eklemeniz gerekir MvcMusicStore.Models ad alanı bildirimi.</span><span class="sxs-lookup"><span data-stu-id="c32f7-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="c32f7-136">Bizim Checkout denetleyicisi üstündeki aşağıda yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="c32f7-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="c32f7-137">CheckoutController aşağıdaki denetleyici eylemleri olacaktır:</span><span class="sxs-lookup"><span data-stu-id="c32f7-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="c32f7-138">**AddressAndPayment (GET method)** bilgilerini girmesini izin vermek için bir form görüntüleyecek.</span><span class="sxs-lookup"><span data-stu-id="c32f7-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="c32f7-139">**AddressAndPayment (POST yöntemini)** giriş doğrulamak ve siparişi işleme.</span><span class="sxs-lookup"><span data-stu-id="c32f7-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="c32f7-140">**Tam** bir kullanıcı teslim alma işlemi başarıyla tamamlandıktan sonra gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="c32f7-141">Bu görünüm kullanıcının sipariş numarası, onay içerir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="c32f7-142">İlk olarak, şimdi (biz denetleyicisini oluşturduğunuzda oluşturulan) dizin denetleyici eylemi için AddressAndPayment yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="c32f7-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="c32f7-143">Bu denetleyici eylemi yalnızca checkout formun görüntüler, böylece herhangi bir model bilgi gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="c32f7-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="c32f7-144">Bizim AddressAndPayment POST yöntemini StoreManagerController kullandık aynı düzeni izler: form gönderme kabul etmek ve sırasını tamamlamak deneyecek ve başarısız olursa formu yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="c32f7-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="c32f7-145">Form girişi doğrulama sipariş bizim doğrulama gereksinimlerini karşılayan sonra biz PromoCode form değer doğrudan denetler.</span><span class="sxs-lookup"><span data-stu-id="c32f7-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="c32f7-146">Her şeyi güncelleştirilmiş bilgileri siparişle kaydedecek doğru olduğunu varsayarak, sipariş sürecini tamamlayıp tam eyleme yönlendirmek için ShoppingCart nesne söyleyin.</span><span class="sxs-lookup"><span data-stu-id="c32f7-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="c32f7-147">Kullanıma alma işlemi başarıyla tamamlandıktan sonra kullanıcılar için tam denetleyici eylemi yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="c32f7-148">Bu eylem, sipariş gerçekten oturum açmış kullanıcıya bir onay olarak sipariş numarası gösterilmeden önce ait olduğunu doğrulamak için basit bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="c32f7-149">*Not: biz proje başladığındaki hata Görünüm otomatik olarak bize /Views/Shared klasöründe oluşturuldu.*</span><span class="sxs-lookup"><span data-stu-id="c32f7-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="c32f7-150">Tam CheckoutController kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="c32f7-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="c32f7-151">AddressAndPayment görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="c32f7-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="c32f7-152">Şimdi, AddressAndPayment görünüm oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="c32f7-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="c32f7-153">Aşağıdakilerden birini sağ AddressAndPayment denetleyici eylemleri ve aşağıda gösterildiği gibi bir sırada kesin türü belirtilmiş ve Düzen şablonunu kullanan AddressAndPayment adlı bir görünüm ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c32f7-153">Right-click on one of the the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="c32f7-154">Bu görünüm yapacak iki biz Aranan adresindeki StoreManagerEdit görünüm oluşturulurken yöntemlerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="c32f7-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="c32f7-155">Html.EditorForModel() sipariş modeli için form alanlarını görüntülemek için kullanacağız</span><span class="sxs-lookup"><span data-stu-id="c32f7-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="c32f7-156">Doğrulama kuralları ile doğrulama öznitelikleri bir sipariş sınıfı kullanarak özelliğinden yararlanır</span><span class="sxs-lookup"><span data-stu-id="c32f7-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="c32f7-157">Promosyon kodu için ek bir metin kutusu tarafından izlenen Html.EditorForModel() kullanmak için form kodu güncelleştirerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="c32f7-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="c32f7-158">AddressAndPayment görünüm için tam kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="c32f7-159">Sipariş için doğrulama kurallarını tanımlama</span><span class="sxs-lookup"><span data-stu-id="c32f7-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="c32f7-160">Bizim görünüm ayarlamak, albüm modeli için daha önce yaptığımız gibi biz doğrulama kurallarını sipariş modelimiz için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c32f7-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="c32f7-161">Modeller klasörü sağ tıklatın ve sipariş adlı bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c32f7-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="c32f7-162">Daha önce albüm için kullandık doğrulama özniteliklerinin yanı sıra, biz de normal bir ifade kullanıcının e-posta adresini doğrulamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="c32f7-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's e-mail address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="c32f7-163">Çalışılıyor eksik formu göndermeden veya geçersiz bilgiler şimdi istemci tarafı doğrulama kullanırken hata iletisi gösterir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="c32f7-164">Tamam, biz kullanıma alma işlemi için sabit işlerin çoğunu yaptıktan; Biz yalnızca birkaç büyük olasılıkla ve tamamlanması uçları sahip.</span><span class="sxs-lookup"><span data-stu-id="c32f7-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="c32f7-165">İki basit görünüm eklemek ihtiyacımız ve oturum açma işlemi sırasında Sepeti bilgi iletimi ilişkin halletmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="c32f7-166">Checkout tam görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="c32f7-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="c32f7-167">Yalnızca sipariş kimliği görüntülenecek gereksinimleriniz değiştikçe Checkout tam görünümü oldukça basit bir işlemdir</span><span class="sxs-lookup"><span data-stu-id="c32f7-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="c32f7-168">Tam denetleyici eylemini sağ tıklayın ve bir tamsayı kesin türü belirtilmiş tam adlı bir görünüm ekleyin</span><span class="sxs-lookup"><span data-stu-id="c32f7-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="c32f7-169">Aşağıda gösterildiği gibi sipariş kimliği, görüntülemek için görünümü kodu şimdi güncelleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="c32f7-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="c32f7-170">Hatanın görünüm güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="c32f7-170">Updating The Error view</span></span>

<span data-ttu-id="c32f7-171">Varsayılan şablonu, bir hata görünümü paylaşılan görünümler klasöründe içerir, başka bir sitede yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c32f7-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="c32f7-172">Bu hata görünümü çok basit bir hata içerir ve biz güncelleştireceğim şekilde sitemizi düzeni kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="c32f7-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="c32f7-173">Bu genel bir hata sayfası olduğundan, içeriği çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="c32f7-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="c32f7-174">Bir ileti ve bağlantı kullanıcı kendi eylem yeniden denemek isterse geçmişi önceki sayfaya gitmek için eklediğimiz.</span><span class="sxs-lookup"><span data-stu-id="c32f7-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="c32f7-175">[Önceki](mvc-music-store-part-8.md)
[sonraki](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="c32f7-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
