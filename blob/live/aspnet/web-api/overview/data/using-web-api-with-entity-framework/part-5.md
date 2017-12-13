---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "Veri aktarımı nesnesi (DTOs) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="6540e-102">Veri aktarımı nesne (DTOs) oluşturma</span><span class="sxs-lookup"><span data-stu-id="6540e-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="6540e-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6540e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6540e-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="6540e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="6540e-105">Sağ istemci veritabanı varlıklara artık, bizim web API kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="6540e-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="6540e-106">İstemci, doğrudan veritabanı tablolarında eşlemeleri veri alır.</span><span class="sxs-lookup"><span data-stu-id="6540e-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="6540e-107">Bununla birlikte, her zaman iyi bir fikir değil.</span><span class="sxs-lookup"><span data-stu-id="6540e-107">However, that's not always a good idea.</span></span> <span data-ttu-id="6540e-108">Bazen istemciye göndermek veri şeklini değiştirmek istersiniz.</span><span class="sxs-lookup"><span data-stu-id="6540e-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="6540e-109">Örneğin, aşağıdakileri yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6540e-109">For example, you might want to:</span></span>

- <span data-ttu-id="6540e-110">Dairesel başvurular (önceki bölüme bakın) kaldırın.</span><span class="sxs-lookup"><span data-stu-id="6540e-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="6540e-111">İstemcileri görüntülemek için görmemesi belirli özelliklerini gizle.</span><span class="sxs-lookup"><span data-stu-id="6540e-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="6540e-112">Yükü boyutunu azaltmak için bazı özellikler atlayın.</span><span class="sxs-lookup"><span data-stu-id="6540e-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="6540e-113">Bunları istemciler için daha kullanışlı hale getirmek için iç içe nesneleri içeren nesne grafiklerinin düzleştirmek.</span><span class="sxs-lookup"><span data-stu-id="6540e-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="6540e-114">"Güvenlik açıkları aşırı gönderim" kaçının.</span><span class="sxs-lookup"><span data-stu-id="6540e-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="6540e-115">(Bkz [Model doğrulama](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) atlayarak nakil Tartışması için.)</span><span class="sxs-lookup"><span data-stu-id="6540e-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="6540e-116">Veritabanı katmanı, hizmet katmanından ayırırsınız.</span><span class="sxs-lookup"><span data-stu-id="6540e-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="6540e-117">Bunu başarmak için tanımlayabilirsiniz bir *veri aktarım nesnesini* (DTO).</span><span class="sxs-lookup"><span data-stu-id="6540e-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="6540e-118">Bir DTO nasıl verileri ağ üzerinden gönderilir tanımlayan bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="6540e-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="6540e-119">Kitap varlıkla nasıl gerçekleştiğine görelim.</span><span class="sxs-lookup"><span data-stu-id="6540e-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="6540e-120">Modeller klasörü iki DTO sınıfları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6540e-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="6540e-121">`BookDetailDTO` Sınıfı kitap modelden hariç özelliklerin tümünü içeren `AuthorName` yazar adı tutacak bir dizedir.</span><span class="sxs-lookup"><span data-stu-id="6540e-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="6540e-122">`BookDTO` Sınıfı içeren bir alt kümesini özelliklerinden `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="6540e-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="6540e-123">Ardından, iki GET yöntemi Değiştir `BooksController` DTOs dönüş sürümleriyle sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6540e-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="6540e-124">LINQ kullanacağız **seçin** defteri varlıklardan DTOs dönüştürmek için deyimi.</span><span class="sxs-lookup"><span data-stu-id="6540e-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="6540e-125">Yeni tarafından oluşturulan SQL işte `GetBooks` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6540e-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="6540e-126">EF LINQ çevirir görebilirsiniz **seçin** SQL SELECT deyimi içinde.</span><span class="sxs-lookup"><span data-stu-id="6540e-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="6540e-127">Son olarak, değişiklik `PostBook` bir DTO döndürülecek yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6540e-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="6540e-128">Bu öğreticide, biz DTOs için el ile kodda dönüştürdüğünüz.</span><span class="sxs-lookup"><span data-stu-id="6540e-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="6540e-129">Gibi bir kitaplık kullanmak üzere başka bir seçenektir [AutoMapper](http://automapper.org/) dönüştürme otomatik olarak yönetir.</span><span class="sxs-lookup"><span data-stu-id="6540e-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6540e-130">[Önceki](part-4.md)
[sonraki](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="6540e-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
