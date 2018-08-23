---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: JavaScript kodu (C#) kullanarak bir denetimi dinamik olarak doldurma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi web hizmetini (veya sayfa yöntemi) çağırır ve t hedef denetime sonuç değerini doldurur...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e8b49f41f132cc31ca458ce0af3b74dbb54f225e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752764"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="9a443-103">JavaScript kodu (C#) kullanarak bir denetimi dinamik olarak doldurma</span><span class="sxs-lookup"><span data-stu-id="9a443-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>
====================
<span data-ttu-id="9a443-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9a443-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9a443-105">[Kodu indir](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9a443-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="9a443-106">ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetimi, bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur.</span><span class="sxs-lookup"><span data-stu-id="9a443-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="9a443-107">Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9a443-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="9a443-108">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="9a443-108">Overview</span></span>

<span data-ttu-id="9a443-109">`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve bir hedef denetimine sayfasında, bir sayfa yenileme olmadan sonuç değerini doldurur.</span><span class="sxs-lookup"><span data-stu-id="9a443-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="9a443-110">Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9a443-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="9a443-111">Adımlar</span><span class="sxs-lookup"><span data-stu-id="9a443-111">Steps</span></span>

<span data-ttu-id="9a443-112">İlk olarak bir ASP.NET Web hizmeti çağrılacak yöntemin uygulayan ihtiyacınız `DynamicPopulateExtender` denetimi.</span><span class="sxs-lookup"><span data-stu-id="9a443-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="9a443-113">Web hizmeti yöntemini uygulayan `getDate()` adlı dize türündeki bir bağımsız değişken bekliyor `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini tek bir parçası olan her web hizmeti çağrısı gönderir.</span><span class="sxs-lookup"><span data-stu-id="9a443-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="9a443-114">Kodu (dosya `DynamicPopulate.cs.asmx`) geçerli tarihi kullanılan üç biçimlerden birini alır:</span><span class="sxs-lookup"><span data-stu-id="9a443-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="9a443-115">Sonraki adımda, yeni bir ASP.NET sitesi oluşturma ve ASP.NET AJAX ScriptManager denetimi ile başlayın:</span><span class="sxs-lookup"><span data-stu-id="9a443-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="9a443-116">Ardından, bir etiket denetimi ekleyin (örneğin aynı ada sahip bir HTML denetimini kullanarak veya `<asp:Label />` web denetimi), daha sonra gösterir web hizmeti çağrısı sonucunu.</span><span class="sxs-lookup"><span data-stu-id="9a443-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="9a443-117">Ardından, içeren bir `DynamicPopulateExtender` denetleyebilir ve web hizmeti bilgileri, hedef denetime, ancak özel JavaScript kullanarak bu yapılır daha sonra popülasyon tetikler denetim adını değil sağlayın!</span><span class="sxs-lookup"><span data-stu-id="9a443-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="9a443-118">Artık JavaScript bölümü.</span><span class="sxs-lookup"><span data-stu-id="9a443-118">Now to the JavaScript part.</span></span> <span data-ttu-id="9a443-119">`$find()` İşlevi, ASP.NET AJAX kitaplığı tarafından tanımlanan sunucu tarafı ASP.NET AJAX Denetim Araç Seti nesnelerin başvurusu gibi döndürür `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="9a443-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="9a443-120">Geçerli dosyadaki `$find("dpe")` bir başvuru döndürür `DynamicPopulateExtender` sayfasındaki denetimi.</span><span class="sxs-lookup"><span data-stu-id="9a443-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="9a443-121">Adlı bir yöntem sunan `populate()` dinamik doldurma işlemi tetikler.</span><span class="sxs-lookup"><span data-stu-id="9a443-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="9a443-122">`populate()` Yöntemi bir bağımsız değişken gerektirir: bağımsız değişken olarak davranacak içerik anahtarı `getDate()` web yöntemini.</span><span class="sxs-lookup"><span data-stu-id="9a443-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="9a443-123">Örneğin, `$find("dpe").populate("format1")` ay gün yıl biçiminde geçerli tarihi etiketle doldurulması.</span><span class="sxs-lookup"><span data-stu-id="9a443-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="9a443-124">Örnek biraz daha esnek hale getirmek için kullanıcı artık çeşitli tarih biçimleri arasında tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9a443-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="9a443-125">Bunların her biri için bir radyo düğmesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9a443-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="9a443-126">Bir kez kullanıcı tıklama radyo düğmesine JavaScript kodu seçilen tarih biçimiyle etiketi dinamik olarak doldurur.</span><span class="sxs-lookup"><span data-stu-id="9a443-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="9a443-127">Bu radyo düğmeleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9a443-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="9a443-128">Bir radyo düğmesi JavaScript ifadesi bağlamında unutmayın `this.value` tam olarak aynı bilgileri özelleştirmede geçerli düğme değerine başvuran `getDate()` yöntemi ile çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="9a443-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="9a443-129">[![Bir düğmeye tıklayın, sunucudan belirtilen biçimde tarihi alır.](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9a443-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="9a443-130">Düğmenin click sunucudan belirtilen biçimde tarihi alır ([tam boyutlu görüntüyü görmek için tıklatın](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9a443-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9a443-131">[Önceki](dynamically-populating-a-control-cs.md)
> [İleri](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9a443-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
