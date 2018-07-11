---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Veritabanına yeni öğe ekleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818514"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="9ca2f-102">Veritabanına yeni öğe ekleme</span><span class="sxs-lookup"><span data-stu-id="9ca2f-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="9ca2f-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9ca2f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9ca2f-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="9ca2f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="9ca2f-105">Bu bölümde, kullanıcıların yeni kitabı oluşturmasını özelliği ekler.</span><span class="sxs-lookup"><span data-stu-id="9ca2f-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="9ca2f-106">App.js içinde görünüm modeli için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9ca2f-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="9ca2f-107">Index.cshtml içinde aşağıdaki biçimlendirme değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9ca2f-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="9ca2f-108">İle:</span><span class="sxs-lookup"><span data-stu-id="9ca2f-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="9ca2f-109">Bu işaretleme, yeni bir yazar göndermek için bir form oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9ca2f-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="9ca2f-110">Yazar aşağı açılan liste değerlerini verilere için bağlı `authors` gözlemlenebilir görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="9ca2f-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="9ca2f-111">Diğer form girişler için değerleri verilere için bağlı `newBook` özelliği görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="9ca2f-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="9ca2f-112">Form üzerinde gönderme işleyicisi bağlı `addBook` işlevi:</span><span class="sxs-lookup"><span data-stu-id="9ca2f-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="9ca2f-113">`addBook` İşlevi, geçerli bir JSON nesnesi oluşturmak için verilere bağlı form girişleri değerlerini okur.</span><span class="sxs-lookup"><span data-stu-id="9ca2f-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="9ca2f-114">JSON nesnesine gönderir sonra `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="9ca2f-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9ca2f-115">[Önceki](part-8.md)
> [İleri](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="9ca2f-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
