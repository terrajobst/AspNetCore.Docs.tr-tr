---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: "7. Kısım: Özellik ekleme | Microsoft Docs"
author: JoeStagner
description: "Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 7 hesabı geçirme gibi ek özellikler ekler..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 5280de44b3e75f9d1ae85e0248bc3ef6d5444f6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-7-adding-features"></a><span data-ttu-id="2846f-104">7. Kısım: Özellikler ekleme</span><span class="sxs-lookup"><span data-stu-id="2846f-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="2846f-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="2846f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="2846f-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2846f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="2846f-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="2846f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="2846f-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="2846f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="2846f-109">Bölüm 7 hesabı gözden geçirme, ürün incelemeleri ve "popüler öğeler" ve "da satın alınan" kullanıcı denetimleri gibi ek özellikler ekler.</span><span class="sxs-lookup"><span data-stu-id="2846f-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a><span data-ttu-id="2846f-110">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="2846f-110">Adding Features</span></span>

<span data-ttu-id="2846f-111">Kullanıcıların bizim katalog göz atabilirsiniz ancak kendi alışveriş sepeti içinde öğeleri yerleştirin ve kullanıma alma işlemini tamamlamak, biz sitemizi geliştirmek için içerecektir destekleme sayısı özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="2846f-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="2846f-112">Hesap gözden geçirme (listesi. siparişleri yerleştirilir ve ayrıntılarına bakın)</span><span class="sxs-lookup"><span data-stu-id="2846f-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="2846f-113">Bazı içeriği belirli içerik ön sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2846f-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="2846f-114">Kullanıcıların gözden geçirme izin vermek için bir özellik katalogdaki ürünlerin ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2846f-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="2846f-115">Denetim popüler öğeler ve Yerleştir ön sayfada görüntülemek için bir kullanıcı denetimi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2846f-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="2846f-116">Bir "Da satın" kullanıcı denetimi oluşturma ve ürün ayrıntıları sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2846f-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="2846f-117">Bir kişi Ekle sayfası.</span><span class="sxs-lookup"><span data-stu-id="2846f-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="2846f-118">Ekleme bir sayfa hakkında.</span><span class="sxs-lookup"><span data-stu-id="2846f-118">Add an About Page.</span></span>
8. <span data-ttu-id="2846f-119">Genel hata</span><span class="sxs-lookup"><span data-stu-id="2846f-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="2846f-120">Hesap gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="2846f-120">Account Review</span></span>

<span data-ttu-id="2846f-121">"Hesap" klasöründe iki .aspx sayfaları bir adlandırılmış OrderList.aspx ve adlandırılmış bir OrderDetails.aspx oluşturun</span><span class="sxs-lookup"><span data-stu-id="2846f-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="2846f-122">Daha önce sahip olduğumuz kadar OrderList.aspx GridView ve EntityDataSoure denetimleri özelliğinden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="2846f-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="2846f-123">Kullanıcı adı özniteliklerinde filtre Siparişler tablosundaki kayıtları EntityDataSoure seçer (WhereParameter bakın) kullanıcı oturumunun kullanıcının bağlandığınızda oturum değişkeni içinde ayarlarız.</span><span class="sxs-lookup"><span data-stu-id="2846f-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="2846f-124">Ayrıca bu GridView HyperlinkField parametrelerinde unutmayın:</span><span class="sxs-lookup"><span data-stu-id="2846f-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="2846f-125">Bu bağlantıyı OrderDetails.aspx sayfa bir sorgu dizesi parametresi olarak OrderID alan belirtme her ürün için sipariş ayrıntılarını görüntülemek için belirtin.</span><span class="sxs-lookup"><span data-stu-id="2846f-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="2846f-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="2846f-126">OrderDetails.aspx</span></span>

<span data-ttu-id="2846f-127">Siparişler bir FormView sipariş verilerini ve başka bir EntityDataSource GridView ile görüntülenecek ve siparişin tüm satır öğeleri görüntülemek için erişmek için bir EntityDataSource denetimini kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2846f-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="2846f-128">Kod arkasında dosyasında (OrderDetails.aspx.cs) temizlik iki az bitleri sahip.</span><span class="sxs-lookup"><span data-stu-id="2846f-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="2846f-129">İlk olarak kimliğinizi Sipariş Ayrıntıları her zaman bir OrderID aldığından emin olmak gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="2846f-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="2846f-130">Biz de hesaplamak ve görüntüleme satır öğelerden toplam sırası gerekir.</span><span class="sxs-lookup"><span data-stu-id="2846f-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="2846f-131">Giriş sayfası</span><span class="sxs-lookup"><span data-stu-id="2846f-131">The Home Page</span></span>

<span data-ttu-id="2846f-132">Bazı statik içerik için Default.aspx sayfasında ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="2846f-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="2846f-133">Önce "İçerik" klasörü oluşturacaksınız ve içerdiği bir görüntü klasörü (ve giriş sayfasında kullanılacak resim eklemek.)</span><span class="sxs-lookup"><span data-stu-id="2846f-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="2846f-134">Aşağıdaki biçimlendirmede Default.aspx sayfasında alt tutucuyu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2846f-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="2846f-135">Ürün incelemeleri</span><span class="sxs-lookup"><span data-stu-id="2846f-135">Product Reviews</span></span>

<span data-ttu-id="2846f-136">Ürün Değerlendirmesi girmek için kullanabileceğiniz bir forma bir bağlantı içeren bir düğme ilk ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2846f-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="2846f-137">Biz sorgu dizesinde ProductID geçtiğiniz unutmayın</span><span class="sxs-lookup"><span data-stu-id="2846f-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="2846f-138">Sonraki sayfa ReviewAdd.aspx adlı ekleyelim</span><span class="sxs-lookup"><span data-stu-id="2846f-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="2846f-139">Bu sayfa, ASP.NET AJAX Denetim Araç Seti kullanır.</span><span class="sxs-lookup"><span data-stu-id="2846f-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="2846f-140">Buradan indirebilirsiniz şekilde, zaten yapmadıysanız, [DevExpress](http://devexpress.com/act) ve burada Visual Studio ile kullanmak için araç seti ayarlama hakkında rehberlik ise [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="2846f-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="2846f-141">Tasarım modunda denetimleri ve doğrulayıcıları Araç Kutusu'ndan sürükleyin ve aşağıdaki gibi bir form oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2846f-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="2846f-142">İşaretleme aşağıdakine benzer görünecektir.</span><span class="sxs-lookup"><span data-stu-id="2846f-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="2846f-143">Biz incelemeler girebilirsiniz, ürün sayfasında bu incelemeleri görüntülemek olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="2846f-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="2846f-144">Bu biçimlendirme ProductDetails.aspx sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2846f-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="2846f-145">Şimdi uygulamamız çalıştıran ve bir ürün için gezinme müşteri incelemelerini ürün bilgilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2846f-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="2846f-146">Popüler öğeler denetimi (kullanıcı denetimleri oluşturma)</span><span class="sxs-lookup"><span data-stu-id="2846f-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="2846f-147">Web sitenizde satışları artırmak için birkaç özellik "müstehcen satış" popüler ya da ilgili ürünler için ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2846f-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="2846f-148">Bu özelliklerin ilk bizim ürün kataloğunda daha popüler ürün listesini olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2846f-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="2846f-149">"Kullanıcı uygulamamız giriş sayfasında öğelerinin satış üstünde görüntülenecek denetimi" oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="2846f-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="2846f-150">Bu denetim olacağından, biz bunu herhangi bir sayfasında yalnızca sürükleyip biz gibi herhangi bir sayfaya Visual Studio'nun Tasarımcısı'nda denetimi bırakarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2846f-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="2846f-151">Visual Studio'nun Çözüm Gezgini'nde, çözüm adına sağ tıklayın ve "Denetimleri" adlı yeni bir dizin oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2846f-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="2846f-152">Bunu yapmak gerekli olmamasına karşın, sizi Projemizin "Denetimleri" dizininde bizim kullanıcı denetimleri oluşturarak tutmaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="2846f-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="2846f-153">Denetimleri klasörü sağ tıklatın ve "Yeni öğesi" seçin:</span><span class="sxs-lookup"><span data-stu-id="2846f-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="2846f-154">"PopularItems" bizim denetimi için bir ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="2846f-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="2846f-155">Kullanıcı denetimleri için dosya uzantısı .ascx değil .aspx olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2846f-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="2846f-156">Bizim popüler öğeleri kullanıcı denetimi şu şekilde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2846f-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="2846f-157">Burada size henüz bu uygulamada kullandık olmayan bir yöntem kullanıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="2846f-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="2846f-158">Yineleyici denetim kullanmakta olduğunuz ve bir veri kaynağı denetimi kullanmak yerine biz yineleyici denetim bir LINQ to Entities sorgusunun sonuçları bağlama.</span><span class="sxs-lookup"><span data-stu-id="2846f-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="2846f-159">Arka plan kod bizim denetiminin içinde aşağıdaki gibi bunu.</span><span class="sxs-lookup"><span data-stu-id="2846f-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="2846f-160">Ayrıca bizim denetimin biçimlendirme üstündeki önemli bu satırı unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2846f-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="2846f-161">En popüler öğeleri bir dakika dakika temelinde değiştirme olmaz bu yana uygulama performansını artırmak için ağrıları getirir yönergesi ekleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="2846f-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="2846f-162">Bu yönerge, önbelleğe alınan çıkış denetimi sona erdiğinde, yalnızca yürütülecek denetimleri kod neden olur.</span><span class="sxs-lookup"><span data-stu-id="2846f-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="2846f-163">Aksi takdirde, denetimin çıkış önbelleğe alınan sürümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2846f-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="2846f-164">Şimdi tüm yapmanız sahibiz bizim yeni denetim Default.aspc sayfamızı içerir.</span><span class="sxs-lookup"><span data-stu-id="2846f-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="2846f-165">Kullanım sürükleyip varsayılan formumuzun açık sütununda denetim örneği yerleştirilecek.</span><span class="sxs-lookup"><span data-stu-id="2846f-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="2846f-166">Şimdi biz giriş sayfası uygulamamız çalıştırdığınızda en popüler öğelerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="2846f-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="2846f-167">"Ayrıca satın" (kullanıcı denetimleri parametrelerle) denetleme</span><span class="sxs-lookup"><span data-stu-id="2846f-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="2846f-168">Oluşturacağız ikinci kullanıcı denetimi müstehcen içerik belirginliğe ekleyerek bir sonraki düzeye satış olur.</span><span class="sxs-lookup"><span data-stu-id="2846f-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="2846f-169">En üstteki "Da satın" öğelerini hesaplamak için mantık Önemsiz olmayan ' dir.</span><span class="sxs-lookup"><span data-stu-id="2846f-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="2846f-170">Bizim "Da satın" denetimi (önceden satın) sipariş ayrıntıları kayıtları için şu anda seçili ProductID seçin ve bulunan her benzersiz sıra OrderIDs alın.</span><span class="sxs-lookup"><span data-stu-id="2846f-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="2846f-171">Ardından biz al ürünleri tüm bu siparişleri ve miktarları satın alınan toplam seçer.</span><span class="sxs-lookup"><span data-stu-id="2846f-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="2846f-172">Biz ürün tarafından miktar toplamı sıralama ve ilk beş öğeleri görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2846f-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="2846f-173">Bu mantık karmaşıklığını verildiğinde, biz bu algoritma bir saklı yordam gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="2846f-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="2846f-174">Saklı yordam için T-SQL aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="2846f-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="2846f-175">Biz, uygulamamız ve biz biz, tabloları ve gerektiğini, varlık veri modeli görünümleri yanı sıra belirtilen varlık veri modeli oluşturulan dahil olduğunda bu saklı yordam (SelectPurchasedWithProducts) veritabanında var olduğunu unutmayın Bu saklı yordam içermelidir.</span><span class="sxs-lookup"><span data-stu-id="2846f-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="2846f-176">Varlık veri modelinin saklı yordam erişmek için şu işlev içeri aktarmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2846f-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="2846f-177">Varlık veri modeli Tasarımcısı'nda açmak ve modeli tarayıcısı açmak için Çözüm Gezgini'nde çift tıklatın, sonra tasarımcıda sağ tıklayın ve "İşlev içeri aktarma Ekle" seçin.</span><span class="sxs-lookup"><span data-stu-id="2846f-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="2846f-178">Bunun yapılması, bu iletişim kutusunu açar.</span><span class="sxs-lookup"><span data-stu-id="2846f-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="2846f-179">Yukarıdaki "SelectPurchasedWithProducts" seçerek gördüğünüz gibi alanları doldurun ve içeri aktarılan bizim işlevin adını yordam adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="2846f-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="2846f-180">"Tamam" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2846f-180">Click "Ok".</span></span>

<span data-ttu-id="2846f-181">Bu size herhangi bir model öğesinde olabileceği gibi biz yalnızca saklı yordam karşı programlama yapabilirsiniz yapılır.</span><span class="sxs-lookup"><span data-stu-id="2846f-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="2846f-182">Bu nedenle, bizim "Denetimleri" klasöründe AlsoPurchased.ascx adlı yeni bir kullanıcı denetimi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2846f-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="2846f-183">Bu denetim için biçimlendirme PopularItems denetimine çok tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="2846f-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="2846f-184">Bu yana işlenmek üzere öğenin ürüne göre farklılık gösterir, çıktıyı önbelleğe değil önemli farktır.</span><span class="sxs-lookup"><span data-stu-id="2846f-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="2846f-185">ProductID denetlemek için "özelliği" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2846f-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="2846f-186">Denetimin PreRender olayını işleyicisinde biz taşmas üç şey yapar.</span><span class="sxs-lookup"><span data-stu-id="2846f-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="2846f-187">ProductID ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="2846f-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="2846f-188">Satın alınan geçerli ürünleriyle olup olmadığına bakın.</span><span class="sxs-lookup"><span data-stu-id="2846f-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="2846f-189">#2'de belirlenen bazı öğeler çıktı.</span><span class="sxs-lookup"><span data-stu-id="2846f-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="2846f-190">Modeli aracılığıyla saklı yordam çağrısı ne kadar kolay olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2846f-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="2846f-191">Var. "da satın alınan" belirlendikten sonra biz yalnızca yineleyici sorgu tarafından döndürülen sonuçlar bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2846f-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="2846f-192">"Ayrıca satın" öğeler olsaydı biz popüler diğer öğeler yalnızca bizim Kataloğu'ndan görüntülersiniz.</span><span class="sxs-lookup"><span data-stu-id="2846f-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="2846f-193">"Ayrıca satın" öğeleri görüntülemek için ProductDetails.aspx sayfasını açın ve böylece bu konumda bir işaretleme görünür AlsoPurchased denetim Çözüm Gezgini'nden sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="2846f-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="2846f-194">Bunun yapılması denetimi başvuru tazelemek sayfanın en üstünde oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2846f-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="2846f-195">AlsoPurchased kullanıcı denetimi ProductID birkaç gerektirdiğinden biz bizim denetiminin ProductID özellik sayfasının geçerli veri modeli öğesi karşı Eval bir deyimi kullanarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2846f-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="2846f-196">Biz yapı ve şimdi çalıştırdığınızda ve bir ürün Gözat biz "Da satın" öğeleri görürler.</span><span class="sxs-lookup"><span data-stu-id="2846f-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

>[!div class="step-by-step"]
<span data-ttu-id="2846f-197">[Önceki](tailspin-spyworks-part-6.md)
[sonraki](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="2846f-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
