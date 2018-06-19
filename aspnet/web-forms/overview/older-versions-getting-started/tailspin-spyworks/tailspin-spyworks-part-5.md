---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '5. Kısım: İş mantığı | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 5 bazı iş mantığı ekler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885087"
---
<a name="part-5-business-logic"></a><span data-ttu-id="7d339-104">5. Kısım: İş mantığı</span><span class="sxs-lookup"><span data-stu-id="7d339-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="7d339-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="7d339-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="7d339-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="7d339-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="7d339-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="7d339-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="7d339-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="7d339-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="7d339-109">Bölüm 5 bazı iş mantığı ekler.</span><span class="sxs-lookup"><span data-stu-id="7d339-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="7d339-110">Bazı iş mantığı ekleme</span><span class="sxs-lookup"><span data-stu-id="7d339-110">Adding Some Business Logic</span></span>

<span data-ttu-id="7d339-111">Birisi web sitemizi ziyaret her kullanılabilmesini alışveriş deneyimi bizim istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="7d339-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="7d339-112">Ziyaretçilerin göz atın ve bunlar olmayan kaydedilmiş veya oturum açmış olsa bile öğeleri alışveriş sepetine eklemek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="7d339-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="7d339-113">Kullanıma hazır olduğunuzda kimliğini doğrulamak için seçeneği sunulur ve değillerse henüz üye bir hesap oluşturmak seçebilecekler.</span><span class="sxs-lookup"><span data-stu-id="7d339-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="7d339-114">Başka bir deyişle, biz "Kayıtlı kullanıcı" durumuna anonim bir durumdan alışveriş sepeti dönüştürmek için mantığı uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7d339-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="7d339-115">Şimdi "Sınıfları" adlı bir dizin oluşturun ardından klasörü sağ tıklatın ve MyShoppingCart.cs adlı yeni bir "Sınıf" dosya oluşturma</span><span class="sxs-lookup"><span data-stu-id="7d339-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="7d339-116">Biz MyShoppingCart.aspx sayfasını uygulayan sınıf genişletme ve bu kullanarak gerçekleştiririz daha önce belirtildiği gibi. NET'in güçlü "kısmi sınıf" oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7d339-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="7d339-117">Bizim MyShoppingCart.aspx.cf dosyası için oluşturulan çağrı şuna benzer.</span><span class="sxs-lookup"><span data-stu-id="7d339-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="7d339-118">"Kısmi" anahtar sözcük kullanımına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7d339-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="7d339-119">Az önce oluşturulan sınıf dosyası şuna benzer.</span><span class="sxs-lookup"><span data-stu-id="7d339-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="7d339-120">Bu dosyaya partial anahtar sözcüğü ekleyerek biz bizim uygulamaları birleştirir.</span><span class="sxs-lookup"><span data-stu-id="7d339-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="7d339-121">Bizim yeni sınıf dosyası şimdi şöyle görünür.</span><span class="sxs-lookup"><span data-stu-id="7d339-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="7d339-122">Bizim sınıfına ekleyeceğiz ilk yöntemi "AddItem" yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="7d339-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="7d339-123">Bu kullanıcı "için resim ekleme" bağlantılar ürün listesi ve ürün ayrıntıları sayfalarında tıkladığında, sonuçta çağrılacağı yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="7d339-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="7d339-124">Kullanarak aşağıdaki append sayfanın üst kısmındaki deyimleri.</span><span class="sxs-lookup"><span data-stu-id="7d339-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="7d339-125">Ve bu yöntem MyShoppingCart sınıfına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7d339-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="7d339-126">Öğe zaten sepetinizde olup olmadığını görmek için LINQ to Entities kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="7d339-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="7d339-127">Bu nedenle, biz miktar öğenin güncelleştirirseniz, aksi takdirde seçili öğe için yeni bir giriş oluşturuyoruz</span><span class="sxs-lookup"><span data-stu-id="7d339-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="7d339-128">Bu yöntemi çağırabilmeniz için yalnızca bu yöntem sınıf ancak öğe eklendikten sonra bir = alışveriş geçerli görüntülenen bir AddToCart.aspx sayfası gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7d339-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="7d339-129">Çözüm Gezgini'nde çözüm adına sağ tıklayın ve eklemek ve biz daha önce yaptığınız gibi AddToCart.aspx adlı yeni bir sayfa.</span><span class="sxs-lookup"><span data-stu-id="7d339-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="7d339-130">Biz bizim uygulamasında düşük stok sorunları, vb., gibi ara sonuçların görüntülenmesi için bu sayfayı kullanabilirsiniz, ancak sayfa gerçekten işlemek, ancak bunun yerine "Ekle" mantığı çağırın ve yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="7d339-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="7d339-131">Bunu gerçekleştirmek için aşağıdaki kod sayfasına ekleyeceğiz\_yük olay.</span><span class="sxs-lookup"><span data-stu-id="7d339-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="7d339-132">Biz bir sorgu dizesi parametresi ve bizim sınıfının addItem yöntemi çağırma alışveriş sepetine eklemek için ürün alınırken unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7d339-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="7d339-133">Hiçbir hata varsayılarak karşılaştı denetim tam olarak biz sonraki gerçekleştireceksiniz SHoppingCart.aspx sayfaya geçirilir.</span><span class="sxs-lookup"><span data-stu-id="7d339-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="7d339-134">Biz bir özel durum bir hata varsa olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7d339-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="7d339-135">Şu anda biz henüz genel hata işleyicisine bu özel durumun uygulamamız tarafından işlenmeyen geçecek ancak Biz bu kısa süre içinde çözebilir uygulanan değil.</span><span class="sxs-lookup"><span data-stu-id="7d339-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="7d339-136">Ayrıca Debug.Fail() (aracılığıyla kullanılabilen deyimi kullanımına dikkat edin `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="7d339-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="7d339-137">Uygulama içinde hata ayıklayıcı çalışıyor, bu yöntem belirttiğimiz hata iletisi ile birlikte uygulamalarının durumu hakkında bilgi içeren ayrıntılı bir iletişim kutusu görüntüler olur.</span><span class="sxs-lookup"><span data-stu-id="7d339-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="7d339-138">Ne zaman Debug.Fail() deyimi üretimde çalışan göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="7d339-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="7d339-139">Bizim alışveriş sepeti sınıf adları "GetShoppingCartId" yöntemine yapılan bir çağrı yukarıda kodda Not.</span><span class="sxs-lookup"><span data-stu-id="7d339-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="7d339-140">Yöntemini aşağıdaki gibi uygulamak için kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7d339-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="7d339-141">Ayrıca güncelleştirme ve checkout düğmeleri ve biz "Toplam" Sepeti burada görüntüleyebilirsiniz etiket ekledik olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7d339-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="7d339-142">Biz şimdi bizim alışveriş sepetine öğeleri ekleyebilir, ancak biz bir ürün eklendikten sonra Sepeti görüntülemek için mantığı uyguladık değil.</span><span class="sxs-lookup"><span data-stu-id="7d339-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="7d339-143">Bu nedenle, MyShoppingCart.aspx sayfasında bir EntityDataSource ve GridVire denetimi gibi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="7d339-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="7d339-144">Böylece güncelleştirme Sepeti düğmesine çift tıklayın ve biçimlendirme bildiriminde belirtilen click olay işleyicisi oluşturmak form tasarımcısında yukarı çağırın.</span><span class="sxs-lookup"><span data-stu-id="7d339-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="7d339-145">Ayrıntılar daha sonra uygulamanız, ancak bunun yapılması oluşturacak bize ve hatasız uygulamamızı çalıştırmak.</span><span class="sxs-lookup"><span data-stu-id="7d339-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="7d339-146">Uygulamayı çalıştırın ve bir öğe alışveriş sepetine eklediğinizde bu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7d339-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="7d339-147">Biz "varsayılan" kılavuz görüntüden üç özel sütunlar uygulayarak deviated olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7d339-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="7d339-148">İlk düzenlenebilir, "Bağlı" alanı miktarı için şöyledir:</span><span class="sxs-lookup"><span data-stu-id="7d339-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="7d339-149">Sonraki satır (öğesi kez sıralanmalıdır maliyet miktar) öğesi toplam "hesaplanan" sütununu şöyledir:</span><span class="sxs-lookup"><span data-stu-id="7d339-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="7d339-150">Son kullanıcının öğesi alışveriş grafikten kaldırılması gerektiğini belirtmek için kullanacağı bir onay kutusu denetimi içeren özel bir sütun vardır.</span><span class="sxs-lookup"><span data-stu-id="7d339-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="7d339-151">Gördüğünüz gibi toplam satır sağlandığından boştur sipariş sipariş toplam hesaplamak için bazı mantık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7d339-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="7d339-152">Biz öncelikle "GetTotal" yöntemi bizim MyShoppingCart sınıf için uygulama.</span><span class="sxs-lookup"><span data-stu-id="7d339-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="7d339-153">MyShoppingCart.cs dosyasına aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7d339-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="7d339-154">Ardından sayfasında\_biz çağırabilir bizim GetTotal yöntemi yük olay işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="7d339-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="7d339-155">Aynı anda alışveriş sepeti boş olup olmadığını ve bu görüntü uygun şekilde ayarlamak için bir test ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="7d339-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="7d339-156">Alışveriş sepeti boşsa, şimdi Biz bu alın:</span><span class="sxs-lookup"><span data-stu-id="7d339-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="7d339-157">Ve aksi durumda, biz bizim toplam bakın.</span><span class="sxs-lookup"><span data-stu-id="7d339-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="7d339-158">Ancak, bu sayfa henüz tamamlanmadı.</span><span class="sxs-lookup"><span data-stu-id="7d339-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="7d339-159">Alışveriş sepeti kaldırılmak üzere işaretlendi öğeleri kaldırma ve bazı kılavuzda kullanıcı tarafından değiştirilmiş olabilecek gibi yeni miktar değerlerini belirleme tarafından yeniden hesaplamak için ilave bir mantık ihtiyacımız.</span><span class="sxs-lookup"><span data-stu-id="7d339-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="7d339-160">Bir kullanıcı bir öğe kaldırma için işaretler olduğunda durumu işlemek için MyShoppingCart.cs bizim alışveriş sepeti sınıfında bir "RemoveItem" yöntem ekleyin olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7d339-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="7d339-161">Şimdi şimdi ad kullanıcı yalnızca GridView sıralanmalıdır kalite değiştirdiğinde koşullar işlemek için bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="7d339-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="7d339-162">Temel özelliklerle Kaldır ve güncelleştirme yerinde gerçekten veritabanında alışveriş sepeti güncelleştirmeleri mantığı uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d339-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="7d339-163">(MyShoppingCart.cs içinde)</span><span class="sxs-lookup"><span data-stu-id="7d339-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="7d339-164">Bu yöntemi iki parametre beklediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7d339-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="7d339-165">Bir alışveriş sepeti kimliği ve diğer kullanıcı tanımlı tür nesneler dizisi.</span><span class="sxs-lookup"><span data-stu-id="7d339-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="7d339-166">Kullanıcı arabirimi özellikleri hakkında mantığımızı bağımlılık en aza indirmek amacıyla alışveriş sepeti öğeleri için kodumuza bizim yöntemi doğrudan erişim GridView denetimi gerek geçirmek için kullanabileceğiniz bir veri yapısı tanımladığınız.</span><span class="sxs-lookup"><span data-stu-id="7d339-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="7d339-167">Bizim MyShoppingCart.aspx.cs dosyasında Biz bu yapı bizim güncelleştirme düğmesini tıklatın olay işleyicisini aşağıdaki gibi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7d339-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="7d339-168">Sepeti güncelleştirmeye ek olarak da Sepeti toplam güncelleştireceğiz olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7d339-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="7d339-169">Bu kod satırı ile özellikle ilgisini çeken dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="7d339-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="7d339-170">GetValues() MyShoppingCart.aspx.cs içinde aşağıdaki gibi uygulayan bir özel yardımcı işlevdir.</span><span class="sxs-lookup"><span data-stu-id="7d339-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="7d339-171">Bu bizim GridView denetimini ilişkili öğeleri değerlerine erişmek için temiz bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="7d339-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="7d339-172">Bizim "Öğeyi kaldır" onay kutusu denetimi bağlı değilse bu yana biz FindControl() yöntemiyle eriştiklerini.</span><span class="sxs-lookup"><span data-stu-id="7d339-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="7d339-173">Bu aşamada, projenizin geliştirme şu anda kullanıma alma işlemini uygulamak için hazır alınıyor.</span><span class="sxs-lookup"><span data-stu-id="7d339-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="7d339-174">Bunu şimdi yaptıktan önce üyelik veritabanı oluşturun ve üyelik depoya bir kullanıcı eklemek için Visual Studio'yu kullanın.</span><span class="sxs-lookup"><span data-stu-id="7d339-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d339-175">[Önceki](tailspin-spyworks-part-4.md)
> [sonraki](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="7d339-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
