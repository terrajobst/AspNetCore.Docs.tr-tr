---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: '3. Kısım: Düzeni ve kategorisi menüsünden | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 3 ekleme düzeni ve kategorisi menüsünden kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882094"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="02b67-104">3. Kısım: Düzeni ve kategori menüsü</span><span class="sxs-lookup"><span data-stu-id="02b67-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="02b67-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="02b67-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="02b67-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="02b67-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="02b67-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="02b67-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="02b67-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="02b67-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="02b67-109">Bölüm 3 ekleme düzeni ve kategorisi menüsünden kapsar.</span><span class="sxs-lookup"><span data-stu-id="02b67-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="02b67-110">Bazı düzeni ve bir kategori menüsü ekleme</span><span class="sxs-lookup"><span data-stu-id="02b67-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="02b67-111">Div bizim ürün kategorisi menüsünden içerecek soluna sütun için bizim site ana sayfasında ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="02b67-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="02b67-112">Bizim Style.css dosyasına eklediğimiz CSS sınıfı tarafından istenen hizalama ve diğer biçimlendirme sağlanacak unutmayın.</span><span class="sxs-lookup"><span data-stu-id="02b67-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="02b67-113">Ürün kategorisi menüsünden, var olan ürün kategorileri ve menü öğelerini oluşturma ve karşılık gelen bağlantıları için ticaret veritabanı sorgulayarak çalışma zamanında dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="02b67-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="02b67-114">Bunu gerçekleştirmek için'de iki ASP kullanacağız. NET'in güçlü veri denetimleri.</span><span class="sxs-lookup"><span data-stu-id="02b67-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="02b67-115">"Varlık veri kaynağı" ve "ListView" denetimi.</span><span class="sxs-lookup"><span data-stu-id="02b67-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="02b67-116">Şimdi "Tasarım görünümüne" ve Yardımcıları bizim denetimleri yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="02b67-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="02b67-117">Şimdi EntityDataSource ID özelliği için EDS ayarlamak\_kategori\_menüsüne ve ardından "Veri kaynağında yapılandırmak".</span><span class="sxs-lookup"><span data-stu-id="02b67-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="02b67-118">Varlık veri kaynağı modeli ticaret Veritabanımıza oluşturduğumuz yükleyen bize için oluşturulmuş CommerceEntities bağlantıyı seçin ve "İleri" yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="02b67-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="02b67-119">"Kategoriler" varlık kümesi adı seçin ve diğer seçenekleri varsayılan olarak bırakın.</span><span class="sxs-lookup"><span data-stu-id="02b67-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="02b67-120">"Son" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="02b67-120">Click "Finish".</span></span>

<span data-ttu-id="02b67-121">Şimdi şimdi biz sayfamızı ListView getirilen ListView denetimi örneği kimliği özelliğini ayarlayın\_ProductsMenu ve kendi Yardımcısını etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="02b67-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="02b67-122">Ancak biz veri öğesi görünümünü biçimlendirmek için denetim seçeneklerini kullanabilirsiniz ve kod kaynağı görünümünde girer şekilde biçimlendirme, bizim menü oluşturma yalnızca basit bir biçimlendirme ister.</span><span class="sxs-lookup"><span data-stu-id="02b67-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="02b67-123">Lütfen "Eval" deyimi unutmayın: &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="02b67-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="02b67-124">ASP.NET sözdizimi &lt;% # %&gt; ne olursa olsun içinde yer alır ve sonuçları "satır" çıktı yürütmek için çalışma zamanı yönlendiren bir toplu kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="02b67-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="02b67-125">Veri öğelerinin, bağlı koleksiyonun geçerli girişi için bildirir Eval("CategoryName") deyimi, varlık modeli öğe adları değerini "CatagoryName" getirin.</span><span class="sxs-lookup"><span data-stu-id="02b67-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="02b67-126">Bu çok güçlü bir özellik kısa sözdizimi aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="02b67-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="02b67-127">Uygulamayı Şimdi Çalıştır olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="02b67-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="02b67-128">Bizim ürün kategorisi menüsünden şimdi görüntülenir ve ProductsList.aspx adlı zaman biz biz menü öğesi bağlantı noktalarına henüz uygulamak için sahip olduğumuz bir sayfa görebilirsiniz kategori menü öğeleri birinin üzerine getirin ve içeren bir dinamik sorgu dizesi bağımsız değişkeni, oluşturduğumuz olduğunu unutmayın  Kategori Kimliği.</span><span class="sxs-lookup"><span data-stu-id="02b67-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="02b67-129">[Önceki](tailspin-spyworks-part-2.md)
> [sonraki](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="02b67-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
