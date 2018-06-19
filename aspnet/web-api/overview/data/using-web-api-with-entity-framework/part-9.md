---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Veritabanına yeni bir öğe ekleyin | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868379"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="223ca-102">Veritabanına yeni bir öğe ekleyin</span><span class="sxs-lookup"><span data-stu-id="223ca-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="223ca-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="223ca-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="223ca-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="223ca-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="223ca-105">Bu bölümde, yeni bir rehberi oluşturmak kullanıcılara ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="223ca-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="223ca-106">App.js içinde görünüm modeli aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="223ca-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="223ca-107">Index.cshtml içinde aşağıdaki biçimlendirme değiştirin:</span><span class="sxs-lookup"><span data-stu-id="223ca-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="223ca-108">İle:</span><span class="sxs-lookup"><span data-stu-id="223ca-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="223ca-109">Bu biçimlendirme yeni Yazar göndermek için bir form oluşturur.</span><span class="sxs-lookup"><span data-stu-id="223ca-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="223ca-110">Yazar aşağı açılan liste değerlerini verilere için bağlı `authors` observable görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="223ca-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="223ca-111">Diğer form girişleri için değerleri veri için bağlı `newBook` görünüm modeli özelliği.</span><span class="sxs-lookup"><span data-stu-id="223ca-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="223ca-112">Formdaki gönderme işleyicisi bağlı `addBook` işlevi:</span><span class="sxs-lookup"><span data-stu-id="223ca-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="223ca-113">`addBook` İşlevi bir JSON nesnesi oluşturmak için veri bağlama form girdi geçerli değerleri okur.</span><span class="sxs-lookup"><span data-stu-id="223ca-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="223ca-114">JSON nesnesi yazılarını sonra `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="223ca-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="223ca-115">[Önceki](part-8.md)
> [sonraki](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="223ca-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
