---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: '3. Bölüm: Düzen ve kategori menüsü | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 3. Bölüm ekleyerek düzen ve kategori menüsü kapsar.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817531"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="4575d-104">3. Bölüm: Düzen ve kategori menüsü</span><span class="sxs-lookup"><span data-stu-id="4575d-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="4575d-105">tarafından [ALi Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4575d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4575d-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4575d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4575d-107">Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="4575d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4575d-108">Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4575d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4575d-109">3. Bölüm ekleyerek düzen ve kategori menüsü kapsar.</span><span class="sxs-lookup"><span data-stu-id="4575d-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="4575d-110">Bazı düzen ve kategori menüsü ekleme</span><span class="sxs-lookup"><span data-stu-id="4575d-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="4575d-111">Bizim ürün kategori menüsü içerecek sol tarafındaki sütunun bir div bizim site ana sayfa ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="4575d-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="4575d-112">Bizim Style.css dosyasına ekledik CSS sınıfı tarafından istenen hizalama ve diğer biçimlendirme sağlanacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4575d-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="4575d-113">Ürün kategorisi menüsünden mevcut ürün kategorilerini ve menü öğeleri oluşturma ve karşılık gelen bağlantıları için ticaret veritabanını sorgulayarak çalışma zamanında dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4575d-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="4575d-114">Bunu gerçekleştirmek için'de iki ASP kullanacağız. NET güçlü veri denetimleri.</span><span class="sxs-lookup"><span data-stu-id="4575d-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="4575d-115">"Varlık veri kaynağı" ve "ListView" denetimi.</span><span class="sxs-lookup"><span data-stu-id="4575d-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="4575d-116">Şimdi "Tasarım görünümüne" ve Yardımcıları bizim denetimlerini yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="4575d-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="4575d-117">EntityDataSource ID özelliği için EDS ayarlayalım\_kategori\_menüsüne ve ardından "Veri kaynağı yapılandırma".</span><span class="sxs-lookup"><span data-stu-id="4575d-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="4575d-118">Ticaret veritabanımızdaki varlık veri kaynağı modeli oluşturduk, bizim için oluşturulmuş CommerceEntities bağlantıyı seçin ve "İleri" ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4575d-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="4575d-119">"Kategoriler" varlık kümesinin adını seçin ve diğer seçenekleri varsayılan olarak bırakın.</span><span class="sxs-lookup"><span data-stu-id="4575d-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="4575d-120">"Son" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="4575d-120">Click "Finish".</span></span>

<span data-ttu-id="4575d-121">Şimdi biz sayfamıza ListView yerleştirilen ListView denetimi örneği ID özelliği ayarlayalım\_ProductsMenu ve kendi Yardımcısı'nı etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4575d-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="4575d-122">Ancak Denetim seçenekleri veri öğesi görünen biçimlendirmek için kullanabiliriz ve biz Kaynak Görünümü'nde kod gireceksiniz biçimlendirme, bizim menü oluşturma yalnızca basit biçimlendirme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4575d-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="4575d-123">"Eval satırını" deyimi lütfen unutmayın: &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="4575d-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="4575d-124">ASP.NET sözdizimini &lt;% # %&gt; ne içerdiği ve sonuçları "satır" çıkış yürütmek için çalışma zamanı yönlendiren bir toplu kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="4575d-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="4575d-125">Bildirir Eval("CategoryName") deyim veri öğelerinin ilişkili koleksiyon geçerli girişi için "CatagoryName" varlık modeli öğe adları değerini getirir.</span><span class="sxs-lookup"><span data-stu-id="4575d-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="4575d-126">Bu çok güçlü bir özellik kısa sözdizimi aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="4575d-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="4575d-127">Şimdi uygulamayı çalıştıralım.</span><span class="sxs-lookup"><span data-stu-id="4575d-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="4575d-128">Bizim ürün kategori menüsü artık görüntülenir ve ProductsList.aspx adlı zaman biz biz menü öğesi bağlantı noktalarına henüz uygulamak için sahip olduğumuz bir sayfa görebilirsiniz kategori menü öğelerinden birinin üzerine gelin ve dinamik sorgu dizesi bağımsız değişkeni, derledik olduğunu içerdiğini unutmayın  Kategori Kimliği.</span><span class="sxs-lookup"><span data-stu-id="4575d-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4575d-129">[Önceki](tailspin-spyworks-part-2.md)
> [İleri](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="4575d-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
