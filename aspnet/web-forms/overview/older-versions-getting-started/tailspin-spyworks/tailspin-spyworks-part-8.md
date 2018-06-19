---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: '8. Kısım: Son sayfaları, özel durum işleme ve sonuç | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölümü 8 sayfa ve özel durum hakkında bir kişi sayfa ekler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: f82294aab0616012393cf3e10f932f6d1ad0cdb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886277"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="298ea-104">8. Kısım: Son sayfaları, özel durum işleme ve sonuç</span><span class="sxs-lookup"><span data-stu-id="298ea-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="298ea-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="298ea-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="298ea-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="298ea-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="298ea-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="298ea-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="298ea-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="298ea-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="298ea-109">Bölümü 8 sayfa ve özel durum işleme hakkında bir kişi sayfa ekler.</span><span class="sxs-lookup"><span data-stu-id="298ea-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="298ea-110">Serinin sonuç budur.</span><span class="sxs-lookup"><span data-stu-id="298ea-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="298ea-111">İlgili kişi sayfası (gönderilen e-posta adresinden ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="298ea-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="298ea-112">ContactUs.aspx adlı yeni bir sayfa oluşturma</span><span class="sxs-lookup"><span data-stu-id="298ea-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="298ea-113">Tasarımcı kullanarak, ToolkitScriptManager ve AjaxdControlToolkit Düzenleyicisi denetiminden dahil olmak üzere özel not alma aşağıdaki form oluşturun.</span><span class="sxs-lookup"><span data-stu-id="298ea-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="298ea-114">biçimindeki telefon numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="298ea-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="298ea-115">Dosyanın arkasındaki kodda bir click olay işleyicisi oluşturun ve ilgili kişi bilgilerini bir e-posta göndermek için bir yöntem uygulamak için "Gönderme" düğmesine çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="298ea-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="298ea-116">Bu kodu web.config dosyasına bir giriş posta göndermek için kullanılacak SMTP sunucusunu belirtir yapılandırma bölümünde içermesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="298ea-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="298ea-117">Sayfa hakkında</span><span class="sxs-lookup"><span data-stu-id="298ea-117">About Page</span></span>

<span data-ttu-id="298ea-118">AboutUs.aspx adlı bir sayfaya oluşturun ve istediğiniz herhangi bir içeriği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="298ea-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="298ea-119">Genel özel durum işleyicisi</span><span class="sxs-lookup"><span data-stu-id="298ea-119">Global Exception Handler</span></span>

<span data-ttu-id="298ea-120">Son olarak, uygulama genelinde şu özel durum ve vardır öngörülemeyen durumlarda bu soğuk Ayrıca web uygulamamız neden işlenmeyen özel durumları.</span><span class="sxs-lookup"><span data-stu-id="298ea-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="298ea-121">Web sitesi ziyaretçisi görüntülenecek işlenmeyen bir özel durum hiçbir zaman istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="298ea-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="298ea-122">İşlenmeyen özel durumlar korkunç kullanıcı deneyimi olan dışında bir güvenlik sorunu da olabilir.</span><span class="sxs-lookup"><span data-stu-id="298ea-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="298ea-123">Bu sorunu çözmek için bir genel özel durum işleyici gerçekleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="298ea-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="298ea-124">Bunu yapmak için Global.asax dosyası açın ve aşağıdaki önceden oluşturulan olay işleyicisini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="298ea-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="298ea-125">Uygulama uygulamak için kodu ekleyin\_şekilde hata işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="298ea-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="298ea-126">Ardından çözüme Error.aspx adlı bir sayfa ekleyin ve bu işaretleme parçacığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="298ea-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="298ea-127">Şimdi sayfasındaki\_olay işleyicisi extract hata iletileri istek nesnesinden yük.</span><span class="sxs-lookup"><span data-stu-id="298ea-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="298ea-128">Sonuç</span><span class="sxs-lookup"><span data-stu-id="298ea-128">Conclusion</span></span>

<span data-ttu-id="298ea-129">ASP.NET WebForms kolaylaştırır olmadığını gördük veritabanı erişimiyle, üyelik, AJAX, Gelişmiş bir Web sitesi oluşturmak için vb.</span><span class="sxs-lookup"><span data-stu-id="298ea-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="298ea-130">oldukça hızlı bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="298ea-130">pretty quickly.</span></span>

<span data-ttu-id="298ea-131">Umarız Bu öğreticiyi kendi ASP.NET WebForms uygulamalar oluşturmaya başlamak için ihtiyacınız olan araçları verdiği!</span><span class="sxs-lookup"><span data-stu-id="298ea-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="298ea-132">Önceki</span><span class="sxs-lookup"><span data-stu-id="298ea-132">Previous</span></span>](tailspin-spyworks-part-7.md)
