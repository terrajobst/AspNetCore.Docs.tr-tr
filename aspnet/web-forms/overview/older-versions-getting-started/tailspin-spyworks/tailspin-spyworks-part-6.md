---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: '6. Bölüm: ASP.NET üyelik | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 6. bölüm ASP.NET üyelik ekler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6d01a745ca1428f065f564f676d483ee807eb52e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368479"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="c9a84-104">6. Bölüm: ASP.NET üyeliği</span><span class="sxs-lookup"><span data-stu-id="c9a84-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="c9a84-105">tarafından [ALi Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="c9a84-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="c9a84-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="c9a84-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="c9a84-107">Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="c9a84-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="c9a84-108">Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c9a84-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="c9a84-109">6. bölüm ASP.NET üyelik ekler.</span><span class="sxs-lookup"><span data-stu-id="c9a84-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="c9a84-110">ASP.NET üyeliği ile çalışma</span><span class="sxs-lookup"><span data-stu-id="c9a84-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="c9a84-111">Güvenlik'e tıklayın</span><span class="sxs-lookup"><span data-stu-id="c9a84-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="c9a84-112">Form kimlik doğrulaması kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="c9a84-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="c9a84-113">Birkaç kullanıcı oluşturmak için "Create User" bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9a84-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="c9a84-114">İşiniz bittiğinde, Çözüm Gezgini penceresinde başvurun ve görünümü yenileyin.</span><span class="sxs-lookup"><span data-stu-id="c9a84-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="c9a84-115">Unutmayın ASPNETDB. MDF ince oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="c9a84-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="c9a84-116">Bu dosya, üyelik gibi temel ASP.NET hizmetlerini desteklemek için tabloları içerir.</span><span class="sxs-lookup"><span data-stu-id="c9a84-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="c9a84-117">Artık kullanıma alma işlemini uygulayan başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c9a84-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="c9a84-118">CheckOut.aspx sayfası oluşturarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="c9a84-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="c9a84-119">CheckOut.aspx sayfası, yalnızca kullanıcılar ve oturum açma sayfasına açmadınız yeniden yönlendirme kullanıcılar kaydedilen biz erişimi kısıtlar için oturum açmış olan kullanıcılar tarafından kullanılabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c9a84-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="c9a84-120">Bunu yapmak için aşağıdaki yapılandırma bölümüne bizim web.config dosyasının ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="c9a84-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="c9a84-121">ASP.NET Web formları uygulamalarını şablon, otomatik olarak bir kimlik doğrulaması bölümü bizim web.config dosyasına eklenir ve varsayılan oturum açma sayfası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c9a84-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="c9a84-122">Biz, kullanıcı oturum açtığında, anonim bir alışveriş sepetini geçirme için dosyanın arkasındaki Login.aspx kodunu değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c9a84-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="c9a84-123">Sayfayı Değiştir\_olay şu şekilde yükleyin.</span><span class="sxs-lookup"><span data-stu-id="c9a84-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="c9a84-124">Ardından bu yeni oturum açan kullanıcının oturum adı ayarlayın ve alışveriş sepetine geçici oturum kimliği, kullanıcının bizim MyShoppingCart sınıfında MigrateCart yöntemi çağırarak değiştirmek için gibi bir "LoggedIn" olayı işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c9a84-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="c9a84-125">(.cs dosya içinde uygulanmış)</span><span class="sxs-lookup"><span data-stu-id="c9a84-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="c9a84-126">Uygulama MigrateCart() yöntemi bu ister.</span><span class="sxs-lookup"><span data-stu-id="c9a84-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="c9a84-127">Alışveriş sepeti sayfamızı yaptığımız kadar checkout.aspx içinde bir EntityDataSource ve GridView bizim kullanıma sayfa içinde kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="c9a84-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="c9a84-128">GridView denetimimiz MyList adlı bir "ondatabound" olay işleyicisi belirtir Not\_RowDataBound Haydi bu olay işleyicisi böyle uygulayın.</span><span class="sxs-lookup"><span data-stu-id="c9a84-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="c9a84-129">Her satır bağlıdır ve en alttaki GridView güncelleştirmeleri alışveriş toplamı sepete yöntemi kalmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="c9a84-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="c9a84-130">Bu aşamada sizi yerleştirilecek sırası "İnceleme" sunumu uyguladınız.</span><span class="sxs-lookup"><span data-stu-id="c9a84-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="c9a84-131">Şimdi bir boş Sepeti senaryosu sayfamıza birkaç satır kod ekleyerek işlemek\_yükleme olayı:</span><span class="sxs-lookup"><span data-stu-id="c9a84-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="c9a84-132">Kullanıcı "Gönder" düğmesine tıkladıktan sonra aşağıdaki kodu Gönder düğmesini tıklatın olay işleyicisi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="c9a84-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="c9a84-133">Sipariş gönderme işlemi "et" bizim MyShoppingCart sınıfının SubmitOrder() yönteminde uygulanması sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="c9a84-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="c9a84-134">SubmitOrder olur:</span><span class="sxs-lookup"><span data-stu-id="c9a84-134">SubmitOrder will:</span></span>

- <span data-ttu-id="c9a84-135">Tüm satır öğeleri alışveriş sepetini alın ve bunları yeni bir sipariş kaydı ve ilişkili OrderDetails kayıtları oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="c9a84-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="c9a84-136">Sevkiyat Tarihi hesaplayın.</span><span class="sxs-lookup"><span data-stu-id="c9a84-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="c9a84-137">Alışveriş sepeti temizleyin.</span><span class="sxs-lookup"><span data-stu-id="c9a84-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="c9a84-138">Bu örnek uygulama amaçları için yalnızca iki gün geçerli tarihe ekleyerek biz bir kullanıma sunulduğundan hesaplayın.</span><span class="sxs-lookup"><span data-stu-id="c9a84-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="c9a84-139">Şimdi uygulamayı çalıştıran bize alışveriş işlemi baştan sona test etmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="c9a84-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9a84-140">[Önceki](tailspin-spyworks-part-5.md)
> [İleri](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="c9a84-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
