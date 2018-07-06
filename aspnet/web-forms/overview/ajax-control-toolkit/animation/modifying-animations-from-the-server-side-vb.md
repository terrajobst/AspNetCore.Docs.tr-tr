---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Sunucu tarafı (VB) tarafından animasyonları değiştirme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonları da olabilir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ff8685b44e7d0c2261ea7b9f1ca6397ae05ce53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811172"
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="a33ac-104">Sunucu tarafı (VB) tarafından animasyonları değiştirme</span><span class="sxs-lookup"><span data-stu-id="a33ac-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="a33ac-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a33ac-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a33ac-106">[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a33ac-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="a33ac-107">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="a33ac-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a33ac-108">Animasyonları, sunucu tarafında değiştirilebilir</span><span class="sxs-lookup"><span data-stu-id="a33ac-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="a33ac-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="a33ac-109">Overview</span></span>

<span data-ttu-id="a33ac-110">ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil.</span><span class="sxs-lookup"><span data-stu-id="a33ac-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a33ac-111">Animasyonları, sunucu tarafında değiştirilebilir</span><span class="sxs-lookup"><span data-stu-id="a33ac-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="a33ac-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="a33ac-112">Steps</span></span>

<span data-ttu-id="a33ac-113">İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:</span><span class="sxs-lookup"><span data-stu-id="a33ac-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="a33ac-114">Animasyonun bir panel şuna benzer metin uygulanır:</span><span class="sxs-lookup"><span data-stu-id="a33ac-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="a33ac-115">İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a33ac-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="a33ac-116">Kodun geri kalanını sunucu tarafında çalışan ve biçimlendirme kullanmaz; Bunun yerine, oluşturmak için kod kullanır `AnimationExtender` denetimi:</span><span class="sxs-lookup"><span data-stu-id="a33ac-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="a33ac-117">Bununla birlikte, Denetim Araç Seti, tek tek animasyon oluşturmak için bir API erişimi şu anda sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="a33ac-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="a33ac-118">Ancak ayarlamak olası `AnimationExtender`projenin bir dizeye animasyon özelliğini kapsayan animasyonları bildirimli olarak atarken kullanılan XML biçimlendirmesi.</span><span class="sxs-lookup"><span data-stu-id="a33ac-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="a33ac-119">Değil içermesi gereken bir XML dosyası oluşturmak için `<Animations>` .NET Framework'ün XML kullanabilirsiniz öğesi desteklemek veya, aşağıdaki kod olduğu gibi yalnızca dize sağlayın:</span><span class="sxs-lookup"><span data-stu-id="a33ac-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="a33ac-120">Son olarak, ekleme `AnimationExtender` içinde geçerli sayfaya, Denetim `<form runat="server">` öğesi, animasyon bulunur ve çalıştırılır emin olun:</span><span class="sxs-lookup"><span data-stu-id="a33ac-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="a33ac-121">[![Animasyon sunucu tarafı C# /VB kod kullanarak oluşturulur.](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a33ac-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="a33ac-122">Animasyon, sunucu tarafı C# /VB kod kullanarak oluşturulur ([tam boyutlu görüntüyü görmek için tıklatın](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a33ac-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a33ac-123">[Önceki](triggering-an-animation-in-another-control-vb.md)
> [İleri](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a33ac-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
