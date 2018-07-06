---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Bunu nasıl yaparım:] AJAX yöntemleri arasında seçin sayfasında güncelleştirmeleri? | Microsoft Docs'
author: JoeStagner
description: Bu videoda, bir ASP.NET uygulamasında AJAX stili Sayfa güncelleştirmelerini gerçekleştirmenin iki birincil yöntem ALi Stagner karşılaştırır. İlk yöntem, bir UDP kullanmaktır...
ms.author: aspnetcontent
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 226fb0423ea05ad9034c909037358331918f2892
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838445"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="1c678-105">[Bunu nasıl yaparım:] AJAX yöntemleri arasında seçin sayfasında güncelleştirmeleri?</span><span class="sxs-lookup"><span data-stu-id="1c678-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="1c678-106">tarafından [ALi Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1c678-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="1c678-107">Bu videoda, bir ASP.NET uygulamasında AJAX stili Sayfa güncelleştirmelerini gerçekleştirmenin iki birincil yöntem ALi Stagner karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="1c678-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="1c678-108">İlk yöntem, hiçbir ek kod istemci tarafı veya sunucu tarafı yazılması gereken yere UpdatePanel, kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1c678-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="1c678-109">UpdatePanel kullanmanın avantajı, her şeyi otomatik olarak çalıştığını ' dir.</span><span class="sxs-lookup"><span data-stu-id="1c678-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="1c678-110">Ceza çok fazla veri AJAX isteği ve yanıt dahil edilmesini gerektirir istemci ve sunucunun yürütülecek bir tam sayfa yaşam döngüsü gerektirir olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="1c678-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="1c678-111">İkinci yöntem, ek kod hem istemci tarafı hem de sunucu tarafı yazılması gereken yere ağ geri çağırmalar kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1c678-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="1c678-112">Ağ geri aramaları kullanmanın avantajı, AJAX isteği ve yanıt dahil edilecek çok az veri gerektiriyorsa istemci ve sunucuda yürütülecek yalnızca çağrılan hizmet yöntemi gerektirir ' dir.</span><span class="sxs-lookup"><span data-stu-id="1c678-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="1c678-113">Penality zaman ve çaba, gerekli kodu yazmak için geçen ' dir.</span><span class="sxs-lookup"><span data-stu-id="1c678-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="1c678-114">ALi, AJAX stili Sayfa güncelleştirmelerini iki birincil yöntem seçerken dikkat etmeniz gereken görüştükten tarafından video sona eriyor.</span><span class="sxs-lookup"><span data-stu-id="1c678-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="1c678-115">(Bu videoyu koddan kullanan [nasıl kullanmaya başlayabilirim ASP.NET AJAX ile](how-do-i-get-started-with-aspnet-ajax.md) video ve [nasıl yaparım? ASP.NET AJAX ile istemci tarafı ağ geri olun](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span><span class="sxs-lookup"><span data-stu-id="1c678-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="1c678-116">&#9654;Videoyu (11 dakika)</span><span class="sxs-lookup"><span data-stu-id="1c678-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="1c678-117">[Önceki](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [İleri](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="1c678-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
