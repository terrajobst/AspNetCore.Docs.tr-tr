---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Öğe ayrıntılarını görüntüleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 29402e70e5fcaac04972788499695ddde4b96531
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832152"
---
<a name="display-item-details"></a><span data-ttu-id="5554e-102">Öğe ayrıntılarını görüntüleme</span><span class="sxs-lookup"><span data-stu-id="5554e-102">Display Item Details</span></span>
====================
<span data-ttu-id="5554e-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5554e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5554e-104">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="5554e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="5554e-105">Bu bölümde, her bir kitabın ayrıntılarını görüntülemek için özelliğini ekleyecektir.</span><span class="sxs-lookup"><span data-stu-id="5554e-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="5554e-106">App.js içinde görünüm modeli aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5554e-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="5554e-107">Views/Home/Index.cshtml içinde veri bağlama öğesi ayrıntıları bağlantısını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5554e-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="5554e-108">Bu tıklama işleyicisine bağlar &lt;bir&gt; öğesine `getBookDetail` işlevi bir görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="5554e-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="5554e-109">Aynı dosyada, aşağıdaki işaretleme değiştirin:</span><span class="sxs-lookup"><span data-stu-id="5554e-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="5554e-110">şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="5554e-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="5554e-111">Bu işaretleme, veri özelliklerine bağlı bir tablo oluşturur `detail` gözlemlenebilir görünüm modeli.</span><span class="sxs-lookup"><span data-stu-id="5554e-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="5554e-112">"&lt;!--Ko--&gt; &quot; DOM öğesinin dışında Knockout bağlama dahil söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="5554e-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="5554e-113">Bu durumda, `if` bağlama neden olur, bu bölümde görüntülenecek biçimlendirme yalnızca `details` null değil.</span><span class="sxs-lookup"><span data-stu-id="5554e-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="5554e-114">Şimdi uygulamayı çalıştırabilir ve birine tıklayın &quot;ayrıntı&quot; bağlantıları, uygulama kitap ayrıntılarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5554e-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="5554e-115">[Önceki](part-7.md)
> [İleri](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="5554e-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
