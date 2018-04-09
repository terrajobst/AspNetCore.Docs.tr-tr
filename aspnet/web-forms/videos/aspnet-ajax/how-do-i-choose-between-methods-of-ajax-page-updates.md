---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Nasıl stop yaparım] AJAX yöntem arasında seçim sayfasında güncelleştirmeleri? | Microsoft Docs'
author: JoeStagner
description: Bu videoda, AJAX stil sayfası güncellemelerini ASP.NET uygulamasını iki birincil yöntemleri Joe Stagner karşılaştırır. İlk yöntem bir Upd kullanmaktır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: b86471b93b7e3a1ed371288195f09fa28353ab36
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="767d6-105">[Nasıl stop yaparım] AJAX yöntem arasında seçim sayfasında güncelleştirmeleri?</span><span class="sxs-lookup"><span data-stu-id="767d6-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>
====================
<span data-ttu-id="767d6-106">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="767d6-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="767d6-107">Bu videoda, AJAX stil sayfası güncellemelerini ASP.NET uygulamasını iki birincil yöntemleri Joe Stagner karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="767d6-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="767d6-108">İlk yöntem nerede ek kod istemci tarafı veya sunucu tarafı yazılması gereken bir UpdatePanel kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="767d6-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="767d6-109">UpdatePanel kullanmanın faydası her şeyi otomatik olarak çalışmasıdır.</span><span class="sxs-lookup"><span data-stu-id="767d6-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="767d6-110">Cezası çok miktarda veri AJAX isteği ve yanıt dahil edilmesini gerektirir istemci ve sunucunun yürütülecek tam sayfa yaşam döngüsü gerektirir olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="767d6-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="767d6-111">İkinci yöntem, burada ek kod hem istemci tarafı hem de sunucu tarafı yazılması gereken ağ geri çağırmalar kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="767d6-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="767d6-112">Ağ geri aramalar kullanmanın faydası, isteğe bağlı olarak istemcide AJAX isteği ve yanıt dahil edilecek çok az veri gerektirir ve sunucuda yürütülecek yalnızca çağrılan hizmet yöntemi gerektirir ' dir.</span><span class="sxs-lookup"><span data-stu-id="767d6-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="767d6-113">Penality zaman ve çaba gerekli kodu yazmak için geçen ' dir.</span><span class="sxs-lookup"><span data-stu-id="767d6-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="767d6-114">Joe AJAX stil sayfası güncelleştirmeleri başlıca iki yöntem arasında seçim yaparken dikkate almanız gerekenler ele video sonlanır.</span><span class="sxs-lookup"><span data-stu-id="767d6-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="767d6-115">(Bu videoyu koddan kullanan [çalışmaya nasıl ASP.NET AJAX ile](how-do-i-get-started-with-aspnet-ajax.md) video ve [nasıl yedeklerim olun istemci-tarafı ağ geri aramalar ASP.NET AJAX ile](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span><span class="sxs-lookup"><span data-stu-id="767d6-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="767d6-116">&#9654;(11 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="767d6-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="767d6-117">[Önceki](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [sonraki](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="767d6-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
