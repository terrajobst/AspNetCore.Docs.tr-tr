---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Bir kullanıcı denetimi ve JavaScript (VB) ile DynamicPopulate kullanarak | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sonuçta elde edilen değerin t hedef denetime doldurur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873007"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="ed421-103">Bir kullanıcı denetimi ve JavaScript (VB) ile DynamicPopulate kullanma</span><span class="sxs-lookup"><span data-stu-id="ed421-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="ed421-104">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ed421-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ed421-105">[Kodu indirme](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ed421-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="ed421-106">ASP.NET AJAX Denetim Araç Seti DynamicPopulate denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve çıkan değeri bir sayfa yenileme olmadan sayfasında hedef denetime doldurur.</span><span class="sxs-lookup"><span data-stu-id="ed421-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="ed421-107">Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ed421-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="ed421-108">Ancak, bir kullanıcı denetimi genişletici bulunduğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="ed421-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="ed421-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="ed421-109">Overview</span></span>

<span data-ttu-id="ed421-110">`DynamicPopulate` ASP.NET AJAX Denetim Araç Seti denetiminde bir web hizmeti (veya sayfa yöntemi) çağırır ve sayfa yenileme olmadan sayfasında hedef denetime sonuç değeri doldurur.</span><span class="sxs-lookup"><span data-stu-id="ed421-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="ed421-111">Özel istemci tarafı JavaScript kodu kullanarak popülasyon tetiklemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ed421-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="ed421-112">Ancak, bir kullanıcı denetimi genişletici bulunduğu olduğunda gerçekleştirilecek özellikle dikkat edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="ed421-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="ed421-113">Adımlar</span><span class="sxs-lookup"><span data-stu-id="ed421-113">Steps</span></span>

<span data-ttu-id="ed421-114">Öncelikle, ASP.NET Web hizmeti çağrılacak yöntemin uygulayan gereksinim `DynamicPopulateExtender` denetim.</span><span class="sxs-lookup"><span data-stu-id="ed421-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="ed421-115">Web hizmeti yöntemi uygulayan `getDate()` adlı dize türünde bir bağımsız değişken bekler `contextKey`, bu yana `DynamicPopulate` denetim bağlam bilgilerini bir parçasını her web hizmeti çağrısı ile gönderir.</span><span class="sxs-lookup"><span data-stu-id="ed421-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="ed421-116">Kod aşağıdaki gibidir (dosyaları `DynamicPopulate.vb.asmx`) üç biçimlerden birinde geçerli tarihi alır:</span><span class="sxs-lookup"><span data-stu-id="ed421-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="ed421-117">Sonraki adımda, yeni bir kullanıcı denetimi oluşturma (`.ascx` dosyası), ilk satırı aşağıdaki bildiriminde tarafından başlar:</span><span class="sxs-lookup"><span data-stu-id="ed421-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="ed421-118">A &lt; `label` &gt; öğesi, sunucudan gelen verileri görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ed421-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="ed421-119">Ayrıca kullanıcı denetimi dosyasında kullanacağız üç radyo düğmeleri, web hizmeti tarafından desteklenen üç olası tarih biçimlerinden birini temsil eden her biri.</span><span class="sxs-lookup"><span data-stu-id="ed421-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="ed421-120">Kullanıcı radyo düğmeleri birinde tıkladığında tarayıcı şöyle JavaScript kodunu yürütün:</span><span class="sxs-lookup"><span data-stu-id="ed421-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="ed421-121">Bu kod erişen `DynamicPopulateExtender` (garip kimliği hakkında endişe etmeyin henüz, bunu daha sonra ele alınacaktır) ve verilerle dinamik popülasyon tetikler.</span><span class="sxs-lookup"><span data-stu-id="ed421-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="ed421-122">Geçerli radyo düğmesi bağlamında `this.value` olan değerine başvuruyor `format1`, `format2` veya `format3` tam olarak hangi web yöntemi bekliyor.</span><span class="sxs-lookup"><span data-stu-id="ed421-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="ed421-123">Kullanıcı denetimi henüz eksik tek şey `DynamicPopulateExtender` radyo düğmeleri web hizmetine bağlayan denetim.</span><span class="sxs-lookup"><span data-stu-id="ed421-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="ed421-124">Yeniden denetiminde kullanılan garip Kimliğini Not: `mcd1$myDate` yerine `myDate`.</span><span class="sxs-lookup"><span data-stu-id="ed421-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="ed421-125">Daha önce kullanılan JavaScript kodu `mcd1_dpe1` erişimi `DynamicPopulateExtender` yerine `dpe1`. Bu adlandırma stratejisi özel gereksinim kullanıldığında `DynamicPopulateExtender` bir kullanıcı denetimi içinde.</span><span class="sxs-lookup"><span data-stu-id="ed421-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="ed421-126">Ayrıca, tüm çalışması için özel bir biçimde kullanıcı öne katıştırmak vardır.</span><span class="sxs-lookup"><span data-stu-id="ed421-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="ed421-127">Yeni bir ASP.NET sayfası oluşturun ve yalnızca uyguladık kullanıcı denetimi için etiket öneki kaydedin:</span><span class="sxs-lookup"><span data-stu-id="ed421-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="ed421-128">Ardından, ASP.NET AJAX dahil `ScriptManager` yeni sayfasında denetimi:</span><span class="sxs-lookup"><span data-stu-id="ed421-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="ed421-129">Son olarak, kullanıcı denetimi sayfasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ed421-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="ed421-130">Yalnızca ayarlamanız gerekir, `ID` özniteliği (ve `runat="server"`, Elbette), ancak belirli bir ad ayarlamak de: `mcd1` bu içinde kullanıcı denetimini JavaScript kullanarak erişmek için kullanılan önek olduğundan.</span><span class="sxs-lookup"><span data-stu-id="ed421-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="ed421-131">Ve bu kadar!</span><span class="sxs-lookup"><span data-stu-id="ed421-131">And that's it!</span></span> <span data-ttu-id="ed421-132">Sayfa beklendiği gibi davranır: kullanıcı radyo düğmeleri birinde tıklarsa, araç seti, denetimi web hizmeti çağrıları ve istenen biçiminde geçerli tarihi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ed421-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="ed421-133">[![Bir kullanıcı denetimi radyo düğmeleri bulunur](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ed421-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="ed421-134">Bir kullanıcı denetimi radyo düğmeleri bulunur ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ed421-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ed421-135">Önceki</span><span class="sxs-lookup"><span data-stu-id="ed421-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
