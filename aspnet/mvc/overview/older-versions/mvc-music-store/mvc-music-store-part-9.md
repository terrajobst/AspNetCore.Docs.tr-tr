---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: '9. Bölüm: Kayıt ve kullanıma alma | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 9. Bölüm kayıt ve kasa işlemleri kapsar.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812879"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="4f2fe-104">9. Bölüm: Kayıt ve kasa işlemleri</span><span class="sxs-lookup"><span data-stu-id="4f2fe-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="4f2fe-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4f2fe-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4f2fe-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4f2fe-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="4f2fe-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4f2fe-109">9. Bölüm kayıt ve kasa işlemleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="4f2fe-110">Bu bölümde, size, platformumuz'ın adresini ve ödeme bilgilerini toplarız CheckoutController oluşturma.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="4f2fe-111">Biz sitemizi kullanıma, önce bu denetleyici yetkilendirme gerektirecek şekilde kaydedin açmasına gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="4f2fe-112">Kullanıcıların kullanıma alma işlemi için bunların alışveriş sepetini "Kullanıma Al" düğmesine tıklayarak gidin.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="4f2fe-113">Kullanıcı oturum açmadığı, için istenir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="4f2fe-114">Başarıyla oturum açtıktan sonra kullanıcı ardından adresinizi ve ödeme görünümü gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="4f2fe-115">Formu doldurduğunuzda varsa ve siparişi gönderen sonra siparişi onay ekranında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="4f2fe-116">Mevcut olmayan bir sipariş ya da size ait olmayan bir sırada görüntülemeye çalıştığınızda hata görünümü gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="4f2fe-117">Alışveriş sepetini geçirme</span><span class="sxs-lookup"><span data-stu-id="4f2fe-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="4f2fe-118">Kullanıcı kullanıma alma düğmeye tıkladığında bir alışveriş işlemi anonim, olsa da, bunlar kaydetme gerekecektir ve oturum açın.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="4f2fe-119">Kullanıcıların, alışveriş sepeti bilgilerine ziyaretleri kayıt veya oturum açma tamamlandığında, alışveriş sepeti bilgileri bir kullanıcıyla ilişkilendirmek ihtiyacımız şekilde korur beklediğiniz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="4f2fe-120">Bizim ShoppingCart sınıfı, geçerli Sepeti tüm öğeleri bir kullanıcı adı ile ilişkilendireceğiniz bir yöntem olduğundan Bunu yapmak gerçekten çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="4f2fe-121">Biz yalnızca bir kullanıcı kaydı veya oturum açma tamamlandığında bu yöntemi çağırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="4f2fe-122">Açık **AccountController** biz üyelik ve yetkilendirme ayarlama yükleyen ekledik sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="4f2fe-123">Ekleme bir using deyimi başvuran MvcMusicStore.Models, ardından aşağıdaki MigrateShoppingCart yöntemi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4f2fe-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="4f2fe-124">Ardından, kullanıcının kimliği doğrulandıktan sonra aşağıda gösterildiği gibi MigrateShoppingCart çağırmak için oturum açma sonrası eylemi değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4f2fe-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="4f2fe-125">Kullanıcı hesabı başarıyla oluşturulduktan hemen sonra eylem sonrası aynı kasaya değişiklik:</span><span class="sxs-lookup"><span data-stu-id="4f2fe-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="4f2fe-126">Bunu - olan artık anonim bir alışveriş sepeti başarılı kayıt veya oturum açma sırasında kullanıcı hesabına otomatik olarak aktarılır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="4f2fe-127">CheckoutController oluşturma</span><span class="sxs-lookup"><span data-stu-id="4f2fe-127">Creating the CheckoutController</span></span>

<span data-ttu-id="4f2fe-128">Denetleyicileri klasörü sağ tıklatın ve boş denetleyici şablonu kullanarak CheckoutController adlı projeye yeni bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="4f2fe-129">İlk olarak, kullanıma alma önce kaydolmalarını iste için denetleyici sınıf bildiriminin üstüne Authorize özniteliği ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4f2fe-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="4f2fe-130">*Not: Bu StoreManagerController için daha önce yaptığımız değişiklik benzer, ancak bu durumda kullanıcı bir yönetici rolünde olmanız Authorize özniteliği gerekli. Kullanıma alma denetleyicide, kullanıcının oturum açmış olmanız biz gerektiren ancak yöneticileri olmaları zorunlu değildir.*</span><span class="sxs-lookup"><span data-stu-id="4f2fe-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="4f2fe-131">Basitleştirmek amacıyla, biz Bu öğreticide ödeme bilgilerle uğraşıyor olmaz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="4f2fe-132">Bunun yerine, biz kullanıcıların bir promosyon kodu kullanarak kullanıma izin vermiş olursunuz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="4f2fe-133">Bu promosyon kodu PromoCode adlı bir sabit kullanarak depolarız.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="4f2fe-134">StoreController olduğu gibi biz storeDB adlı MusicStoreEntities sınıfının bir örneğini tutacak bir alan bildirmeniz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="4f2fe-135">Yapmak için MusicStoreEntities sınıfı kullanın, biz kullanarak bir eklemeniz gerekecektir MvcMusicStore.Models ad alanı bildirimi.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="4f2fe-136">Kullanıma alma denetleyicimizin en altında görünür.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="4f2fe-137">Şu denetleyici eylemleri CheckoutController olacaktır:</span><span class="sxs-lookup"><span data-stu-id="4f2fe-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="4f2fe-138">**AddressAndPayment (GET method)** bilgilerini girmesini izin vermek için bir form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="4f2fe-139">**AddressAndPayment (POST yöntemi)** girişini doğrulamak ve siparişi işleme.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="4f2fe-140">**Tam** kullanıcı kullanıma alma işlemi başarıyla tamamlandıktan sonra gösterilir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="4f2fe-141">Bu görünüm, kullanıcının sipariş numarası da onay olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="4f2fe-142">İlk olarak, şimdi AddressAndPayment için (denetleyici oluşturduğumuz bağlandığınızda oluşturuldu) dizin denetleyici eylemini yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="4f2fe-143">Herhangi bir model bilgi gerektirmez, dolayısıyla bu denetleyici eylemi yalnızca kullanıma alma form görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="4f2fe-144">Bizim AddressAndPayment POST yöntemi içinde StoreManagerController kullandık aynı düzeni izler: form gönderme kabul edin ve sırasını tamamlamak deneyecek ve başarısız olursa formu yeniden görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="4f2fe-145">Form girişini doğrulama sipariş bizim doğrulama gereksinimlerini karşılayan sonra biz doğrudan PromoCode form değeri kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="4f2fe-146">Her şeyin doğru olduğunu varsayarsak, biz güncelleştirilmiş bilgileri siparişle kaydedebilir, sipariş işlemini tamamlayıp tamamlama eyleminde olduğu için yeniden yönlendirme ShoppingCart söylemek.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="4f2fe-147">Kullanıma alma işlemi işlemin başarıyla tamamlanmasından sonra kullanıcılar için eksiksiz bir denetleyici eylemini yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="4f2fe-148">Bu eylem sırasını gerçekten oturum açmış kullanıcıya bir onay olarak sipariş numarası göstermeden önce ait olduğunu doğrulamak için basit bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="4f2fe-149">*Not: şu projenin başladığında hata görünümünün otomatik olarak bizim için /Views/Shared klasöründe oluşturulur.*</span><span class="sxs-lookup"><span data-stu-id="4f2fe-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="4f2fe-150">Tam CheckoutController kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="4f2fe-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="4f2fe-151">AddressAndPayment görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="4f2fe-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="4f2fe-152">Şimdi, AddressAndPayment görünüm oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="4f2fe-153">AddressAndPayment denetleyici eylemleri birine sağ tıklayın ve aşağıda gösterildiği gibi bir sırada kesin ve düzenleme şablonu kullanıldığı AddressAndPayment adlı bir görünüm ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="4f2fe-154">Bu görünüm hale getirecek iki incelemiştik adresindeki StoreManagerEdit görünümü oluştururken tekniklerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4f2fe-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="4f2fe-155">Sipariş modeli için form alanlarını görüntülemek için Html.EditorForModel() kullanacağız</span><span class="sxs-lookup"><span data-stu-id="4f2fe-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="4f2fe-156">Bir sipariş sınıfı ile doğrulama öznitelikleri kullanarak doğrulama kuralları yararlanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="4f2fe-157">Html.EditorForModel(), ek bir textbox tarafından izlenen promosyon kodunu kullanmak için form kodu güncelleştirerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="4f2fe-158">AddressAndPayment görünümü için tam kod aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="4f2fe-159">Sipariş için doğrulama kurallarını tanımlama</span><span class="sxs-lookup"><span data-stu-id="4f2fe-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="4f2fe-160">Bizim görünümü ayarlamak, albüm modeli için daha önce yaptığımız gibi doğrulama kuralları sipariş modelimizi ayarlayacağız.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="4f2fe-161">Modeller klasörü sağ tıklatın ve sipariş adlı bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="4f2fe-162">Albüm için daha önce kullandığımız doğrulama özniteliklerinin yanı sıra, biz de normal bir ifade kullanıcının e-posta adresinizi doğrulamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="4f2fe-163">Çalışılıyor formunun eksik olan veya geçersiz bilgileri artık kullanarak istemci tarafı doğrulama hata iletisi gösterir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="4f2fe-164">Tamam, çoğunu sabit kullanıma alma işlemi için uyguladığımız güncelleştirmede; yalnızca birkaç ekledikçe ve tamamlanması uçları sahibiz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="4f2fe-165">İki basit görünüm eklemek ihtiyacımız ve oturum açma işlemi sırasında sepet bilgi iletimi ölçeklendirilmesini gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="4f2fe-166">Kullanıma alma tam görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="4f2fe-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="4f2fe-167">Yalnızca sipariş kimliği görüntülenecek gereksinimleriniz değiştikçe kullanıma alma tam görünümü oldukça basit</span><span class="sxs-lookup"><span data-stu-id="4f2fe-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="4f2fe-168">Tam denetleyici eylemini sağ tıklayıp, bir tamsayı kesin tam olarak adlandırılan bir görünümü Ekle</span><span class="sxs-lookup"><span data-stu-id="4f2fe-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="4f2fe-169">Aşağıda gösterildiği gibi sipariş kimliği görüntülenecek görünüm kodu artık güncelleştireceğiz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="4f2fe-170">Hatanın görünüm güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="4f2fe-170">Updating The Error view</span></span>

<span data-ttu-id="4f2fe-171">Böylece, başka bir site için yeniden kullanılabilir varsayılan şablonu paylaşılan görünümler klasöründe bir hata görünümü içerir.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="4f2fe-172">Bu hata görünüm çok basit bir hata içeriyor ve bu güncelleştireceğiz şekilde sitemizi düzeni kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="4f2fe-173">Bu genel bir hata sayfası olduğundan, içeriği çok kolaydır.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="4f2fe-174">Bir ileti ve bir bağlantı kullanıcı kendi eylemini yeniden denemek isterse geçmişinde önceki sayfaya gitmek için yer alacak.</span><span class="sxs-lookup"><span data-stu-id="4f2fe-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="4f2fe-175">[Önceki](mvc-music-store-part-8.md)
> [İleri](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="4f2fe-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
