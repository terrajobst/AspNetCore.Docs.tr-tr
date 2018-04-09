---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: '2. Kısım: Veri erişim katmanı | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 2. kısım, veri erişim katmanı ekleme kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="961fc-104">2. Kısım: Veri erişim katmanı</span><span class="sxs-lookup"><span data-stu-id="961fc-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="961fc-105">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="961fc-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="961fc-106">Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="961fc-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="961fc-107">Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.</span><span class="sxs-lookup"><span data-stu-id="961fc-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="961fc-108">Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="961fc-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="961fc-109">2. kısım, veri erişim katmanı ekleme kapsar.</span><span class="sxs-lookup"><span data-stu-id="961fc-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="961fc-110">Veri erişim katmanı ekleme</span><span class="sxs-lookup"><span data-stu-id="961fc-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="961fc-111">E-ticaret uygulamamız iki veritabanlarında bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="961fc-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="961fc-112">Standart ASP.NET üyelik veritabanı için müşteri bilgileri kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="961fc-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="961fc-113">Bizim alışveriş sepeti ve ürün kataloğu için şu SQL Express veritabanı gibi uygulamanız.</span><span class="sxs-lookup"><span data-stu-id="961fc-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="961fc-114">Veritabanı (Commerce.mdf) uygulamanın uygulamada oluşturduktan\_ilerlemeden .NET Entity Framework kullanarak bizim veri erişim katmanı oluşturmak için veri klasörü.</span><span class="sxs-lookup"><span data-stu-id="961fc-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="961fc-115">Adlı bir klasör oluşturacağız "veri\_erişim" ve bunları bu klasörü sağ tıklatın ve "Yeni Öğe Ekle" seçin.</span><span class="sxs-lookup"><span data-stu-id="961fc-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="961fc-116">"Yüklenmiş şablonlarda" öğesini ve ardından "ADO.NET varlık veri modeli" EDM girin\_Commerce.edmx adı olarak ve "Ekle" düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="961fc-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="961fc-117">"Veritabanı oluştur"'i seçin.</span><span class="sxs-lookup"><span data-stu-id="961fc-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="961fc-118">Oluşturma ve kaydedin.</span><span class="sxs-lookup"><span data-stu-id="961fc-118">Save and build.</span></span>

<span data-ttu-id="961fc-119">Şimdi biz ilk özelliğimizi – ürün kategorisi menüsünden eklemek hazır olursunuz.</span><span class="sxs-lookup"><span data-stu-id="961fc-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="961fc-120">[Önceki](tailspin-spyworks-part-1.md)
> [sonraki](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="961fc-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
