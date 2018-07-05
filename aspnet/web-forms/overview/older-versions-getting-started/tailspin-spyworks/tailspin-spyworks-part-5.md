---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '5. Bölüm: İş mantığı | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 5. Bölüm bazı iş mantığı ekler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: 719d56e0764e2f66b8813c9487119bbc700d738c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377992"
---
<a name="part-5-business-logic"></a><span data-ttu-id="00285-104">5. Bölüm: İş mantığı</span><span class="sxs-lookup"><span data-stu-id="00285-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="00285-105">tarafından [ALi Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="00285-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="00285-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="00285-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="00285-107">Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="00285-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="00285-108">Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="00285-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="00285-109">5. Bölüm bazı iş mantığı ekler.</span><span class="sxs-lookup"><span data-stu-id="00285-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="00285-110">Bazı iş mantığı ekleme</span><span class="sxs-lookup"><span data-stu-id="00285-110">Adding Some Business Logic</span></span>

<span data-ttu-id="00285-111">Alışveriş deneyimimizi birisi web sitemizi ziyaret ne zaman kullanılabilir olmasını istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="00285-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="00285-112">Ziyaretçi göz atın ve bunlar katılmamaları kayıtlı veya oturum açmış olsanız bile, alışveriş sepetine öğe ekleme mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="00285-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="00285-113">Kullanıma hazır olduğunuzda kimliğini doğrulama seçeneği sunulur ve değillerse henüz üyeleri, bir hesap oluşturmak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="00285-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="00285-114">Başka bir deyişle, biz alışveriş sepetini anonim bir durumdan bir "Kullanıcı kayıtlı" duruma dönüştürmek için mantığı uygulamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="00285-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="00285-115">Şimdi "Sınıfları" adlı bir dizin oluşturmak ardından klasörüne sağ tıklayıp MyShoppingCart.cs adlı yeni bir "Class" dosya oluşturma</span><span class="sxs-lookup"><span data-stu-id="00285-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="00285-116">Daha önceden belirtildiği biz MyShoppingCart.aspx sayfası uygulayan sınıf genişletme ve bu kullanarak yapacağız. NET güçlü "kısmi Class" yapısı.</span><span class="sxs-lookup"><span data-stu-id="00285-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="00285-117">Bizim MyShoppingCart.aspx.cf dosya için oluşturulan çağrı aşağıdaki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="00285-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="00285-118">"Kısmi" anahtar sözcüğü kullanımına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="00285-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="00285-119">Az önce oluşturulan sınıf dosyası şöyle görünür.</span><span class="sxs-lookup"><span data-stu-id="00285-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="00285-120">Partial anahtar sözcüğü bu dosyaya ekleyerek size sunduğumuz uygulamaları birleştirecektir.</span><span class="sxs-lookup"><span data-stu-id="00285-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="00285-121">Sunduğumuz yeni bir sınıf dosyası artık şöyle görünür.</span><span class="sxs-lookup"><span data-stu-id="00285-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="00285-122">Bizim sınıfına ekleyeceğiz ilk yöntem "AddItem" yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="00285-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="00285-123">Ürün listesi ve ürün ayrıntıları sayfalarında "Resim Ekle" bağlantılarında kullanıcı tıkladığında, sonuçta çağrılacak yöntem budur.</span><span class="sxs-lookup"><span data-stu-id="00285-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="00285-124">Aşağıdakileri kullanarak ekleme deyimini sayfasının üst.</span><span class="sxs-lookup"><span data-stu-id="00285-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="00285-125">Ve bu yöntem MyShoppingCart sınıfına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="00285-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="00285-126">Öğenin zaten arabasında olup olmadığını görmek için LINQ to Entities kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="00285-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="00285-127">Bu nedenle, miktar öğesinin güncelleştirmemiz durumunda, aksi takdirde seçili öğe için yeni bir giriş oluştururuz</span><span class="sxs-lookup"><span data-stu-id="00285-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="00285-128">Bu yöntem çağırmak için yalnızca bu yöntem sınıf ancak ardından öğesi eklendikten sonra bir = alışveriş geçerli görüntülenen bir AddToCart.aspx sayfası uygular.</span><span class="sxs-lookup"><span data-stu-id="00285-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="00285-129">Çözüm Gezgini'nde çözümün adına sağ tıklayın ve eklemek ve daha önce uyguladığımız olarak AddToCart.aspx adlı yeni bir sayfa.</span><span class="sxs-lookup"><span data-stu-id="00285-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="00285-130">Bu sayfada kararlılığımızın içinde düşük stok sorunları, vb., geçiş sonuçları görüntülemek için kullanabiliriz ancak sayfa gerçekten işlemek, ancak bunun yerine "Ekle" mantıksal çağrı ve yeniden yönlendirme.</span><span class="sxs-lookup"><span data-stu-id="00285-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="00285-131">Bunu gerçekleştirmek için aşağıdaki kod sayfasına ekleyeceğiz\_yükleme olayı.</span><span class="sxs-lookup"><span data-stu-id="00285-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="00285-132">Biz bir sorgu dizesi parametresini ve bizim sınıfının addItem yöntemi çağırma alışveriş sepetine eklemek için ürün almakta olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00285-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="00285-133">Hiçbir hata olmadığı kabul edilerek karşılaştı denetimi tamamen size sonraki uygulayacak SHoppingCart.aspx sayfasına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="00285-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="00285-134">Biz, bir hata yoksa bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="00285-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="00285-135">Şu anda henüz genel hata işleyicisi bu özel durumun uygulamamız tarafından işlenmemiş gitmesi gerekiyordu ancak Biz bu kısa bir süre içinde çözebilir uyguladık değil.</span><span class="sxs-lookup"><span data-stu-id="00285-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="00285-136">Ayrıca Debug.Fail() (aracılığıyla kullanılabilir deyimi kullanımına dikkat edin `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="00285-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="00285-137">Olan uygulamayı hata ayıklayıcısı içinde çalışırken, bu yöntem belirttiğimiz hata iletisi ile birlikte uygulama durumu hakkındaki bilgileri içeren ayrıntılı bir iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="00285-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="00285-138">Üretimde deyimi yoksayıldı Debug.Fail() çalışırken.</span><span class="sxs-lookup"><span data-stu-id="00285-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="00285-139">Bizim alışveriş sepeti sınıf adları "GetShoppingCartId" bir yönteme bir çağrı üzerindeki kodda fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="00285-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="00285-140">Yöntemi gibi uygulamak için kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="00285-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="00285-141">Ayrıca güncelleştirme ve kullanıma alma düğmeleri ve biz "Toplam" sepete burada görüntüleyebilirsiniz etiket ekledik olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00285-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="00285-142">Biz artık alışveriş sepetimizi öğeleri ekleyebilir, ancak bir ürün eklendikten sonra sepet görüntülenecek mantıksal uyguladık değil.</span><span class="sxs-lookup"><span data-stu-id="00285-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="00285-143">Bu nedenle, MyShoppingCart.aspx sayfasında EntityDataSource denetimini ve GridVire gibi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="00285-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="00285-144">Form Tasarımcısı'nda ' kurmak alışveriş sepetini güncelleştir düğmesine çift tıklayın ve biçimlendirme bildiriminde belirtilen click olay işleyicisi oluşturmak için çağırın.</span><span class="sxs-lookup"><span data-stu-id="00285-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="00285-145">Ayrıntılar daha sonra uygulama ancak bunun yapılması oluşturacak bize ve hatasız uygulamamızı çalıştırmak.</span><span class="sxs-lookup"><span data-stu-id="00285-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="00285-146">Uygulamayı çalıştırmak ve alışveriş sepetine öğe ekleme görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="00285-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="00285-147">Biz "varsayılan" kılavuz görüntüden üç özel sütunlar uygulayarak deviated olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00285-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="00285-148">İlk bir düzenlenebilir, "Bağlı" alanı miktarı için verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="00285-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="00285-149">Sonraki satır (sipariş edilmesi gereken miktar kez öğesi maliyet) öğesi toplam "hesaplanan" sütununu şöyledir:</span><span class="sxs-lookup"><span data-stu-id="00285-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="00285-150">Son kullanıcının öğe alışveriş şemasından kaldırılması gerektiğini belirtmek için kullanacağı bir CheckBox denetimi içeren özel bir sütun vardır.</span><span class="sxs-lookup"><span data-stu-id="00285-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="00285-151">Gördüğünüz gibi toplam satır şimdi boş siparişin sipariş toplam hesaplamak için bazı mantık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="00285-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="00285-152">Bir "GetTotal" yöntem ilk bizim MyShoppingCart sınıfına uygulayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="00285-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="00285-153">MyShoppingCart.cs dosyasına aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="00285-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="00285-154">Ardından sayfasında\_Load olay işleyicisinde biz çağırabilir bizim GetTotal yöntemi.</span><span class="sxs-lookup"><span data-stu-id="00285-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="00285-155">Aynı anda alışveriş sepetini boş olup olmadığını ve bu görüntü uygun şekilde ayarlamak için bir test ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="00285-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="00285-156">Alışveriş sepeti boşsa, artık bu aldığımız:</span><span class="sxs-lookup"><span data-stu-id="00285-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="00285-157">Ve aksi takdirde, bizim toplam görüyoruz.</span><span class="sxs-lookup"><span data-stu-id="00285-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="00285-158">Ancak, bu sayfa henüz tamamlanmadı.</span><span class="sxs-lookup"><span data-stu-id="00285-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="00285-159">Alışveriş sepeti kaldırılmak üzere işaretlendi öğeleri kaldırarak ve bazı kılavuzda kullanıcı tarafından değiştirilmiş olabilecek yeni miktar değerlerini belirleyen ile yeniden hesaplamak için ilave bir mantık ihtiyacımız.</span><span class="sxs-lookup"><span data-stu-id="00285-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="00285-160">Bir kullanıcı bir öğeyi kaldırma işaretler, durumu işlemek için MyShoppingCart.cs bizim alışveriş sepeti sınıfında "da removeItem" yöntemi eklemek olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="00285-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="00285-161">Şimdi github'dan bir kullanıcı yalnızca içinde GridView sıralanmalıdır kalitesi değiştiğinde durumda işlemek için bir yöntem ad.</span><span class="sxs-lookup"><span data-stu-id="00285-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="00285-162">Temel özelliklerle Kaldır ve güncelleştirme yerinde gerçekten veritabanında alışveriş sepetini güncelleştirme mantığı uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="00285-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="00285-163">(MyShoppingCart.cs içinde)</span><span class="sxs-lookup"><span data-stu-id="00285-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="00285-164">Bu yöntem, iki parametre bekliyor unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00285-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="00285-165">Bir alışveriş sepeti kimliği ve diğer kullanıcı tanımlı türü nesnelerin dizisi.</span><span class="sxs-lookup"><span data-stu-id="00285-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="00285-166">Bağımlılık bizim mantığı kullanıcı arabirimi özellikleri hakkında en aza indirmek için alışveriş sepetine öğe kodumuz için sunduğumuz yöntemi GridView denetiminde doğrudan bağlanmasına gerek geçirmek için kullanabileceğiniz bir veri yapısı tanımladınız.</span><span class="sxs-lookup"><span data-stu-id="00285-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="00285-167">Bizim MyShoppingCart.aspx.cs dosyasında bu yapı bizim güncelleştirme düğmesine tıklayın olay işleyicisi aşağıdaki gibi kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="00285-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="00285-168">Alışveriş sepetini güncelleştirme yanı sıra biz de sepet toplam güncelleştirileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00285-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="00285-169">Bu kod satırı ile yakından unutmayın:</span><span class="sxs-lookup"><span data-stu-id="00285-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="00285-170">GetValues() MyShoppingCart.aspx.cs içinde şu şekilde uygulayan bir özel yardımcı işlevdir.</span><span class="sxs-lookup"><span data-stu-id="00285-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="00285-171">Bu, bizim GridView denetimindeki ilişkili öğe değerlerini erişmek için temiz bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="00285-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="00285-172">Bizim "Öğeyi kaldır" onay kutusu denetimi bağlı değilse bu yana biz FindControl() yöntemi erişirsiniz.</span><span class="sxs-lookup"><span data-stu-id="00285-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="00285-173">Bu, projenizin geliştirme aşamasında size kullanıma alma işlemini uygulamak hazır hazırlanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="00285-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="00285-174">Şimdi bunu önce üyelik veritabanı oluşturun ve üyelik depoya bir kullanıcı eklemek için Visual Studio'yu kullanın.</span><span class="sxs-lookup"><span data-stu-id="00285-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00285-175">[Önceki](tailspin-spyworks-part-4.md)
> [İleri](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="00285-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
