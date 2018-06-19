---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: '4. Kısım: Ürünleri listeleme | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 4 GridView Sözl listeleme ürünleriyle kapsayan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881070"
---
<a name="part-4-listing-products"></a><span data-ttu-id="e86da-104">4. Kısım: Listeleme ürünleri</span><span class="sxs-lookup"><span data-stu-id="e86da-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="e86da-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e86da-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e86da-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="e86da-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e86da-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="e86da-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e86da-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="e86da-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e86da-109">Bölüm 4 GridView denetimi ile listeleme ürünleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="e86da-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="e86da-110">GridView denetiminin ürünleriyle listeleme</span><span class="sxs-lookup"><span data-stu-id="e86da-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="e86da-111">"Sağ tıklayarak" ProductsList.aspx sayfamızı uygulama çözümümüzdür ve "Ekle" ve "Yeni öğesi" seçme başlayalım.</span><span class="sxs-lookup"><span data-stu-id="e86da-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="e86da-112">"Web formu kullanarak ana sayfa" seçin ve ProductsList.aspx sayfa adını girin".</span><span class="sxs-lookup"><span data-stu-id="e86da-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="e86da-113">"Ekle" yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e86da-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="e86da-114">Sonraki biz Site.Master sayfa nereye yerleştirileceğini "Stilleri" klasörü seçin ve "Klasörünün içeriğini" penceresinden seçin.</span><span class="sxs-lookup"><span data-stu-id="e86da-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="e86da-115">Sayfayı oluşturmak için "Tamam"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e86da-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="e86da-116">Veritabanımıza aşağıda görüldüğü gibi ürün verilerle doldurulur.</span><span class="sxs-lookup"><span data-stu-id="e86da-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="e86da-117">Sayfamızı oluşturulduktan sonra yeniden bir varlık veri kaynağı bu ürün verilere erişmek için kullanacağız ancak bu örnek Ürün varlıklar seçmeliyiz ve yalnızca seçilen kategori için döndürülürsünüz öğeleri kısıtlamak ihtiyacımız.</span><span class="sxs-lookup"><span data-stu-id="e86da-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="e86da-118">Bunu gerçekleştirmek için otomatik oluşturmak için EntityDataSource WHERE yan tümcesi anlatılır ve WhereParameter belirtmeniz.</span><span class="sxs-lookup"><span data-stu-id="e86da-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="e86da-119">Menü öğeleri bizim "ürün kategorisi menüde" oluşturduğumuz zaman dinamik olarak bağlantı her bağlantı için sorgu dizesi CatagoryID ekleyerek oluşturduğumuz olduğunu Hatırlayacağınız.</span><span class="sxs-lookup"><span data-stu-id="e86da-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="e86da-120">Biz bu sorgu dizesi parametresi WHERE parametre çıkarmaya varlık veri kaynağı söyler.</span><span class="sxs-lookup"><span data-stu-id="e86da-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="e86da-121">Ardından, ürünlerin listesini görüntülemek için ListView denetimi yapılandıracağız.</span><span class="sxs-lookup"><span data-stu-id="e86da-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="e86da-122">En iyi bir alışveriş deneyimi oluşturmak için size çeşitli kısa özellikler bizim ListVew görüntülenen her ürünün düzenlenecek.</span><span class="sxs-lookup"><span data-stu-id="e86da-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="e86da-123">Ürün adı, ürünün ayrıntı görünümü için bir bağlantı olur.</span><span class="sxs-lookup"><span data-stu-id="e86da-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="e86da-124">Ürünün fiyat görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e86da-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="e86da-125">Ürün görüntüsü görüntülenir ve biz dinamik olarak görüntünün bir kataloğu resimleri dizinden bizim uygulamada seçersiniz.</span><span class="sxs-lookup"><span data-stu-id="e86da-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="e86da-126">Biz hemen ürününün alışveriş sepetine eklemek için bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="e86da-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="e86da-127">Bizim ListView denetimi örneği için biçimlendirme aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="e86da-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="e86da-128">Biz, dinamik olarak görüntülenen her ürün için birkaç bağlantıları oluşturmakta olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="e86da-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="e86da-129">Ayrıca, kendi yeni sayfa test ederiz önce biz ürün için dizin yapısını Kataloğu resimleri şu şekilde oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e86da-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="e86da-130">Bizim ürün görüntüleri erişilebilir olduğunda size ürün listesi sayfamızı test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e86da-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="e86da-131">Sitenin giriş sayfasından kategori listesi bağlantılardan birini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e86da-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="e86da-132">Şimdi biz ProductDetials.apsx sayfası ve AddToCart işlevselliği uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e86da-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="e86da-133">Dosya - kullanın&gt;sayfa adı daha önce yaptığımız gibi sitenin ana sayfa kullanarak ProductDetails.aspx oluşturmak için yeni.</span><span class="sxs-lookup"><span data-stu-id="e86da-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="e86da-134">Biz yeniden EntityDataSource denetim veritabanında belirli bir ürün kaydı erişmek için kullanır ve bir ASP.NET FormView denetimi gibi ürün verileri görüntülemek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="e86da-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="e86da-135">Biçimlendirme biraz size komik değilse endişelenmeyin.</span><span class="sxs-lookup"><span data-stu-id="e86da-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="e86da-136">Yukarıdaki biçimlendirme görüntüleme düzeni odada birkaç biz daha sonra uygulama özelliklerinin bırakır.</span><span class="sxs-lookup"><span data-stu-id="e86da-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="e86da-137">Alışveriş sepetine uygulamamız daha karmaşık mantık temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e86da-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="e86da-138">Başlamak için dosya - kullanın&gt;MyShoppingCart.aspx adlı bir sayfa oluşturmak için yeni.</span><span class="sxs-lookup"><span data-stu-id="e86da-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="e86da-139">Biz ShoppingCart.aspx adı almadığınızdan unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e86da-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="e86da-140">Veritabanımıza "Alışveriş sepeti" adlı bir tablo içeriyor.</span><span class="sxs-lookup"><span data-stu-id="e86da-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="e86da-141">Biz bir varlık veri modeli oluşturulan seçtiğinizde bir sınıf veritabanındaki her tablo için oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e86da-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="e86da-142">Bu nedenle, varlık veri modeli "Alışveriş sepeti" adlı bir varlık sınıfı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e86da-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="e86da-143">Model böylece biz bizim alışveriş sepeti uygulaması için bu adı kullanın veya bizim ihtiyaçları için genişletme ancak biz yalnızca farklı çakışmayı önlemek adı yerine benimseyeceksiniz düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e86da-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="e86da-144">Ayrıca biz basit alışveriş sepeti oluşturacağınız belirtmeye ve alışveriş sepeti mantığı ile alışveriş sepeti görüntüle katıştırma değer değildir.</span><span class="sxs-lookup"><span data-stu-id="e86da-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="e86da-145">Biz alışveriş sepetimizi tamamen ayrı bir iş Katmanı'nı uygulamak seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e86da-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e86da-146">[Önceki](tailspin-spyworks-part-3.md)
> [sonraki](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="e86da-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
