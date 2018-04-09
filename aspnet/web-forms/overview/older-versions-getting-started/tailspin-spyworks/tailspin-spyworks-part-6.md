---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Bölüm 6: ASP.NET üyelik | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 6 ASP.NET üyelik ekler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="768d6-104">Bölüm 6: ASP.NET üyelik</span><span class="sxs-lookup"><span data-stu-id="768d6-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="768d6-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="768d6-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="768d6-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="768d6-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="768d6-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="768d6-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="768d6-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="768d6-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="768d6-109">Bölüm 6 ASP.NET üyelik ekler.</span><span class="sxs-lookup"><span data-stu-id="768d6-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="768d6-110">ASP.NET üyelik ile çalışma</span><span class="sxs-lookup"><span data-stu-id="768d6-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="768d6-111">Güvenlik'e tıklayın</span><span class="sxs-lookup"><span data-stu-id="768d6-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="768d6-112">Form kimlik doğrulaması kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="768d6-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="768d6-113">Birkaç kullanıcı oluşturmak için "Kullanıcı oluştur" bağlantısını kullanın.</span><span class="sxs-lookup"><span data-stu-id="768d6-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="768d6-114">İşiniz bittiğinde, Çözüm Gezgini penceresine bakın ve görünümü yenileyin.</span><span class="sxs-lookup"><span data-stu-id="768d6-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="768d6-115">Unutmayın ASPNETDB. MDF ince oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="768d6-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="768d6-116">Bu dosya üyelik gibi çekirdek ASP.NET Hizmetleri desteklemek için tabloları içerir.</span><span class="sxs-lookup"><span data-stu-id="768d6-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="768d6-117">Artık kullanıma alma işlemini uygulama başlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="768d6-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="768d6-118">CheckOut.aspx sayfası oluşturarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="768d6-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="768d6-119">CheckOut.aspx sayfası yalnızca kullanıcılar ve oturum açma sayfasına açmadınız yeniden yönlendirme kullanıcılar oturum biz erişimi kısıtlar şekilde oturum açmış kullanıcılar için kullanılabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="768d6-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="768d6-120">Bunu yapmak için aşağıdaki bizim web.config dosyasını yapılandırma bölümü ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="768d6-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="768d6-121">Şablon ASP.NET Web Forms uygulamaları için otomatik olarak bir kimlik doğrulaması bölümü bizim web.config dosyasına eklendi ve varsayılan oturum açma sayfasına kuruldu.</span><span class="sxs-lookup"><span data-stu-id="768d6-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="768d6-122">Biz, kullanıcı oturum açtığında anonim bir alışveriş sepeti geçirmek için dosyanın arkasındaki Login.aspx kodu değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="768d6-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="768d6-123">Sayfa değiştirme\_olay aşağıdaki gibi yükleyin.</span><span class="sxs-lookup"><span data-stu-id="768d6-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="768d6-124">Ardından bu yeni oturum açan kullanıcının oturum adı ve alışveriş sepeti geçici oturum kimliği, kullanıcının bizim MyShoppingCart sınıfında MigrateCart yöntemini çağırarak değiştirmek için gibi bir "LoggedIn" olay işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="768d6-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="768d6-125">(.Cs dosyasında uygulanır)</span><span class="sxs-lookup"><span data-stu-id="768d6-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="768d6-126">MigrateCart() yöntemini uygulayın bu ister.</span><span class="sxs-lookup"><span data-stu-id="768d6-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="768d6-127">Alışveriş sepeti sayfamızı yaptığımız kadar checkout.aspx içinde bir EntityDataSource ve GridView bizim kullanıma sayfa içinde kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="768d6-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="768d6-128">Bizim GridView denetim MyList adlı bir "ondatabound" olay işleyicisi belirtir Not\_RowDataBound sağlandığından, olay işleyicisi şöyle uygulayın.</span><span class="sxs-lookup"><span data-stu-id="768d6-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="768d6-129">Bu yöntem alışveriş toplamı Sepeti her satır bağlıdır ve en alttaki GridView güncelleştirmeleri gibi kalmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="768d6-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="768d6-130">Bu aşamada biz yerleştirilecek sırasını "gözden geçirme" sunumu uyguladık.</span><span class="sxs-lookup"><span data-stu-id="768d6-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="768d6-131">Şimdi bir boş Sepeti senaryosu sayfamızı için birkaç satır kod ekleyerek işlemek\_yük olay:</span><span class="sxs-lookup"><span data-stu-id="768d6-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="768d6-132">Kullanıcı "Gönderme" düğmesine tıkladığında aşağıdaki kod gönder düğmesine tıklayın olay işleyicisini yürütülür.</span><span class="sxs-lookup"><span data-stu-id="768d6-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="768d6-133">Bizim MyShoppingCart sınıfının SubmitOrder() yöntemi uygulanacak sipariş gönderme işleminin "et" dir.</span><span class="sxs-lookup"><span data-stu-id="768d6-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="768d6-134">SubmitOrder aşağıdakileri yapar:</span><span class="sxs-lookup"><span data-stu-id="768d6-134">SubmitOrder will:</span></span>

- <span data-ttu-id="768d6-135">Alışveriş sepeti tüm satır öğeleri alır ve bunları yeni bir sipariş kaydı ve ilişkili sipariş ayrıntıları kayıtları oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="768d6-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="768d6-136">Sevkiyat Tarihi hesaplayın.</span><span class="sxs-lookup"><span data-stu-id="768d6-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="768d6-137">Alışveriş sepeti temizleyin.</span><span class="sxs-lookup"><span data-stu-id="768d6-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="768d6-138">Bu örnek uygulama amaçları doğrultusunda basitçe geçerli tarihe kadar iki gün ekleyerek biz sevk tarihi hesaplama.</span><span class="sxs-lookup"><span data-stu-id="768d6-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="768d6-139">Uygulamayı şimdi çalıştıran başlangıçtan bitişe kadar alışveriş işlemini test etmek için bize izin verir.</span><span class="sxs-lookup"><span data-stu-id="768d6-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="768d6-140">[Önceki](tailspin-spyworks-part-5.md)
> [sonraki](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="768d6-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
