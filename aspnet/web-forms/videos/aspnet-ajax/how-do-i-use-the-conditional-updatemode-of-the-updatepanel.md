---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Nasıl stop yaparım] UpdatePanel, koşullu UpdateMode kullanılsın mı? | Microsoft Docs'
author: JoeStagner
description: ASP.NET AJAX UpdatePanel 'Her zaman' veya 'Koşullu' olarak ayarlanmış bir UpdateMode özelliği içerir. Her zaman, UpdatePan durumda varsayılandır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/01/2007
ms.topic: article
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: 45416f101e6e85060f68c594192cef35503bcfed
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884788"
---
<a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="e39da-105">[Nasıl stop yaparım] UpdatePanel, koşullu UpdateMode kullanılsın mı?</span><span class="sxs-lookup"><span data-stu-id="e39da-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>
====================
<span data-ttu-id="e39da-106">tarafından [CAN Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e39da-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="e39da-107">ASP.NET AJAX UpdatePanel 'Her zaman' veya 'Koşullu' olarak ayarlanmış bir UpdateMode özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="e39da-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="e39da-108">Her zaman, bu durumda UpdatePanel her zaman içeriğini bir asychronous geri gönderme sırasında güncelleştirecektir varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="e39da-108">The default is Always, in which case the UpdatePanel will always update its content during an asychronous postback.</span></span> <span data-ttu-id="e39da-109">Bu videoda biz bizim sunucu tarafı kodu, güncelleştirme yöntemini çağırdığında, servis talebi UpdatePanel yalnızca içeriği güncelleştirir koşullu UpdateMode nasıl ayarlayacağınızı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="e39da-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="e39da-110">Bu UpdatePanel içeriğini geçerli zaman uyumsuz geri gönderme sırasında güncelleştirme olup olmadığını belirlemek için C# veya Visual Basic kodu koşullu mantık kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="e39da-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="e39da-111">&#9654;(13 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="e39da-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="e39da-112">[Önceki](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [sonraki](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="e39da-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
