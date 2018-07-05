---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: '7. Bölüm: Özellik ekleme | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 7. Bölüm hesabı geçirme gibi ek özellikler ekler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389182"
---
<a name="part-7-adding-features"></a><span data-ttu-id="2cd2e-104">7. Bölüm: Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="2cd2e-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="2cd2e-105">tarafından [ALi Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="2cd2e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="2cd2e-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="2cd2e-107">Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="2cd2e-108">Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="2cd2e-109">7. Bölüm hesabı gözden geçirme, ürün incelemeleri ve "popüler öğeler" ve "Ayrıca satın alınan" kullanıcı denetimleri gibi ek özellikler ekler.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="2cd2e-110">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="2cd2e-110">Adding Features</span></span>

<span data-ttu-id="2cd2e-111">Kullanıcılar kataloğumuzu göz atabilirsiniz ancak öğe, alışveriş sepetine içinde yerleştirin ve kullanıma alma işlemini tamamlamak, biz sitemizi geliştirmek için içerecektir destekleme birkaç özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="2cd2e-112">Hesap gözden geçirme (listesi. siparişler yerleştirilir ve ayrıntılarına bakın)</span><span class="sxs-lookup"><span data-stu-id="2cd2e-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="2cd2e-113">Bazı bağlam belirli bir içeriğe ön sayfaya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="2cd2e-114">Kullanıcıların gözden geçirme sağlamak için bir özellik ürün kataloğunda ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="2cd2e-115">Popüler öğeler ve yerde denetleyen ön sayfada görüntülemek için bir kullanıcı denetimi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="2cd2e-116">"Ayrıca satın alınan" bir kullanıcı denetimi oluşturun ve ürün ayrıntıları sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="2cd2e-117">Bir kişi Ekle sayfası.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="2cd2e-118">Ekleme bir sayfa hakkında.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-118">Add an About Page.</span></span>
8. <span data-ttu-id="2cd2e-119">Genel hata</span><span class="sxs-lookup"><span data-stu-id="2cd2e-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="2cd2e-120">Hesap gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="2cd2e-120">Account Review</span></span>

<span data-ttu-id="2cd2e-121">"Hesap" klasöründe bir adlandırılmış OrderList.aspx ve adlandırılmış bir OrderDetails.aspx iki .aspx sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="2cd2e-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="2cd2e-122">Daha önce sahip olduğumuz kadar OrderList.aspx GridView ve EntityDataSoure denetimleri özelliğinden yararlanır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="2cd2e-123">Kullanıcı adı özniteliklerinde filtrelenmiş Siparişler tablosundan kayıtları EntityDataSoure seçer (WhereParameter bakın) kullanıcı oturumunun kullanıcının, bir oturum değişkeni içinde ayarlarız.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="2cd2e-124">Ayrıca bu parametreleri GridView'ın HyperlinkField dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="2cd2e-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="2cd2e-125">Bu bağlantı ayrıntılarını görüntüleyebilmek OrderDetails.aspx sayfasına bir sorgu dizesi parametresi olarak OrderID alan belirtilmesi her ürün için belirtin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="2cd2e-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="2cd2e-126">OrderDetails.aspx</span></span>

<span data-ttu-id="2cd2e-127">Siparişler ve tüm sipariş satır öğeleri görüntülemek için bir FormView'da sipariş verilerini ve GridView ile başka bir EntityDataSource görüntülenecek erişmek için bir EntityDataSource denetimini kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="2cd2e-128">Kod dosyasında (OrderDetails.aspx.cs) iki biraz temizlik bitlerini sahibiz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="2cd2e-129">İlk olarak biz OrderDetails her zaman bir OrderID alır emin olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="2cd2e-130">Ayrıca hesaplamak ve satır öğeleri toplam sırayı görüntülemek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="2cd2e-131">Giriş sayfası</span><span class="sxs-lookup"><span data-stu-id="2cd2e-131">The Home Page</span></span>

<span data-ttu-id="2cd2e-132">Bazı statik içeriklerin Default.aspx sayfasına ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="2cd2e-133">İlk "İçerik" klasörünü oluşturacaksınız ve içerdiği bir Resimler klasörü (ve giriş sayfasında kullanılacak bir görüntü yer alacak.)</span><span class="sxs-lookup"><span data-stu-id="2cd2e-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="2cd2e-134">Default.aspx sayfasında alt tutucusu aşağıdaki işaretlemeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="2cd2e-135">Ürün incelemeleri</span><span class="sxs-lookup"><span data-stu-id="2cd2e-135">Product Reviews</span></span>

<span data-ttu-id="2cd2e-136">Ürün gözden geçirmesi girmek için kullanabileceğiniz bir forma ilk bağlantısını içeren bir düğme ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="2cd2e-137">ProductID sorgu dizesinde geçiriyoruz olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="2cd2e-138">Sonraki sayfa ReviewAdd.aspx adlı ekleyelim</span><span class="sxs-lookup"><span data-stu-id="2cd2e-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="2cd2e-139">Bu sayfa, ASP.NET AJAX Denetim Araç Seti kullanır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="2cd2e-140">Buradan indirebileceğiniz şekilde, zaten yapmadıysanız, [DevExpress](http://devexpress.com/act) ve araç seti Visual Studio ile burada kullanmak için ayarlama hakkında yönergeler ise [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="2cd2e-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="2cd2e-141">Tasarım modunda denetimler ve doğrulayıcılar araç kutusundan sürükleyin ve aşağıdaki gibi bir form oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="2cd2e-142">Biçimlendirme, aşağıdaki gibi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="2cd2e-143">Biz incelemeleri girebilirsiniz, ürün sayfasında bu incelemeleri görüntülemek olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="2cd2e-144">Bu işaretleme ProductDetails.aspx sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="2cd2e-145">Şimdi uygulamamız çalıştıran ve bir ürün için gezinme müşteri incelemeleri de dahil olmak üzere ürün bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="2cd2e-146">Sık kullanılan öğe denetimi (kullanıcı denetimleri oluşturma)</span><span class="sxs-lookup"><span data-stu-id="2cd2e-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="2cd2e-147">Web sitenizde satışları artırmak için birkaç özellik "müstehcen sell" popüler veya ilgili ürünler ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="2cd2e-148">Bu özelliklerin ilk ürün kataloğu daha popüler ürün listesi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="2cd2e-149">"Kullanıcı satış uygulamamız giriş sayfasındaki öğelerin üst görüntülenecek denetimi" oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="2cd2e-150">Bu denetim olacağından, bunu herhangi bir sayfa üzerinde yalnızca sürükleyip istiyoruz herhangi bir sayfaya Visual Studio'nun Tasarımcısı'nda denetim bırakarak kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="2cd2e-151">Visual Studio Çözüm Gezgini'nde, çözüm adına sağ tıklayın ve "Controls" adlı yeni bir dizin oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="2cd2e-152">Bunu yapmak gerekli olmamasına karşın, biz "Controls" dizininde bizim kullanıcı denetimleri oluşturarak Projemizin kalmasına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="2cd2e-153">Denetimleri klasörü sağ tıklatın ve "Yeni öğe" seçin:</span><span class="sxs-lookup"><span data-stu-id="2cd2e-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="2cd2e-154">"PopularItems" bizim denetimi için bir ad belirtin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="2cd2e-155">Kullanıcı denetimleri için dosya uzantısı .ascx değil .aspx olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="2cd2e-156">Popüler öğeleri kullanıcı denetimimiz şu şekilde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="2cd2e-157">Burada henüz bu uygulamada kullandık olmayan bir yöntem kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="2cd2e-158">Repeater denetimiyle kullanıyoruz ve bir veri kaynağı denetimi kullanmak yerine Repeater denetiminde bir LINQ to Entities sorgusunda sonuçlarını bağlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="2cd2e-159">Arka plan kod bizim denetiminin içinde şu şekilde desteklemiyoruz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="2cd2e-160">Ayrıca, bu önemli satır üst bizim denetimin biçimlendirme unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="2cd2e-161">En popüler öğeleri, bir dakika için dakika başına değiştirme olmaz beri uygulamamız performansını artırmak için bir ağrıları getirir yönergesi ekleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="2cd2e-162">Bu yönerge, önbelleğe alınan çıkış denetimi süresi dolduğunda, yalnızca yürütülecek denetimleri koda neden olur.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="2cd2e-163">Aksi takdirde, denetimin çıkışı önbelleğe alınmış sürümünü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="2cd2e-164">Biz yapmanız gereken tek şey artık yeni denetimimiz Default.aspc sayfamızı içerir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="2cd2e-165">Kullanım sürükleyip varsayılan formumuzun Aç sütununda bir denetim örneği yerleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="2cd2e-166">Şimdi uygulamamız giriş sayfasında ne zaman çalıştırıyoruz, en popüler öğeleri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="2cd2e-167">"Ayrıca satın alınan" Denetim (parametrelerle kullanıcı denetimleri)</span><span class="sxs-lookup"><span data-stu-id="2cd2e-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="2cd2e-168">İkinci kullanıcı denetimi oluşturacağız müstehcen içerik belirginliğe ekleyerek bir sonraki düzeye satış olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="2cd2e-169">"Ayrıca satın alınan" üst öğeleri hesaplamak için mantık önemsiz değil.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="2cd2e-170">"Ayrıca satın alınan" denetimimiz (önceden satın alınan) OrderDetails kayıtları için şu anda seçili ProductID seçin ve bulunan her bir benzersiz siparişi için OrderIDs alın.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="2cd2e-171">Ardından al ürünleri tüm bu siparişleri ve miktarları satın alınan toplam seçiyoruz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="2cd2e-172">Biz ürünleri miktar toplamı sıralamak ve ilk beş öğeleri görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="2cd2e-173">Bu mantık karmaşıklığını göz önünde bulundurulduğunda, biz bir saklı yordam bu algoritma uygular.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="2cd2e-174">Saklı yordam için T-SQL aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="2cd2e-175">Biz bunu uygulamamız ve biz size, tablolar ve görünümler varlık veri modeli, ihtiyacımız, ek olarak belirtilen varlık veri modeli oluşturulan eklendiğinde bu saklı yordamı (SelectPurchasedWithProducts) veritabanında var olduğunu unutmayın. Bu saklı yordamı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="2cd2e-176">Varlık veri modeli saklı yordamı erişmek için biz işlevi aktarmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="2cd2e-177">Varlık veri modeli Tasarımcısı'nda açın ve Model tarayıcı açmak için Çözüm Gezgini'nde çift tıklayın, sonra tasarımcıda sağ tıklayın ve "İşlevi Al Ekle"'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="2cd2e-178">Bunun yapılması, bu iletişim kutusunu açar.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="2cd2e-179">Yukarıdaki "SelectPurchasedWithProducts" seçerek gördüğünüz gibi alanları doldurun ve yordam adı için sunduğumuz içeri aktarılan bir işlevin adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="2cd2e-180">"Tamam" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-180">Click "Ok".</span></span>

<span data-ttu-id="2cd2e-181">Bu size herhangi bir öğeyi model, olabileceği gibi biz karşı saklı yordamı yalnızca programlayabileceğiniz yapılır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="2cd2e-182">Bu nedenle bizim "Controls" klasöründe AlsoPurchased.ascx adlı yeni bir kullanıcı denetimi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="2cd2e-183">Bu denetim için biçimlendirme PopularItems denetime çok tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="2cd2e-184">İşlenecek öğenin ürüne göre değişir olduğundan, çıktıyı önbelleğe değil önemli fark vardır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="2cd2e-185">ProductID denetlemek için "özelliği" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="2cd2e-186">Denetimin PreRender olayını işleyicisi şu üç şeyi yapmak için eed.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="2cd2e-187">ProductID ayarlandığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="2cd2e-188">Satın alınan geçerli ürünleriyle olup olmadığına bakın.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="2cd2e-189">#2'de belirlendiği bazı öğeler çıktı.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="2cd2e-190">Modeli aracılığıyla saklı yordam çağrısı ne kadar kolay olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="2cd2e-191">Belirledikten sonra burada "Ayrıca satın alınan" biz yalnızca yineleyici sorgu tarafından döndürülen sonuçlar bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="2cd2e-192">"Ayrıca satın alınan" tüm öğeler ise yalnızca diğer popüler öğeler bizim kataloğundan görüntüleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="2cd2e-193">"Ayrıca satın alınan" öğeleri görüntülemek için ProductDetails.aspx sayfasını açın ve biçimlendirme içinde bu konumda görünmesi Çözüm Gezgini'nden AlsoPurchased denetimi sürükleyin.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="2cd2e-194">Bunun yapılması tazelemek sayfanın en üstündeki denetimin bir başvuru oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="2cd2e-195">ProductID birkaç AlsoPurchased kullanıcı denetimini gerektirdiğinden denetimimiz ProductID özelliğini bir sayfanın geçerli veri modeli öğesi Eval deyimini kullanarak ayarlayacağız.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="2cd2e-196">Oluşturmamızı ve şimdi çalıştırmak ve ürüne göz atın, "Ayrıca satın alınan" öğeleri görüyoruz.</span><span class="sxs-lookup"><span data-stu-id="2cd2e-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="2cd2e-197">[Önceki](tailspin-spyworks-part-6.md)
> [İleri](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="2cd2e-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
