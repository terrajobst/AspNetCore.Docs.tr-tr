---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[Nasıl yedeklerim]: bir kullanıcı denetimi durumunu geri gönderme sırasında kalıcı | Microsoft Docs'
author: rick-anderson
description: Bu video Chris Pels, bir kullanıcı denetimi bir veya daha fazla nesne durumu devam ettirmek gösterilmiştir. İlk olarak, bir kullanıcı denetimi abilit temsil eden oluşturulur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572097"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Nasıl yedeklerim]: bir kullanıcı denetimi durumunu geri gönderme sırasında Sürdür
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="9d3d4-105">tarafından [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="9d3d4-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="9d3d4-106">Bu video Chris Pels, bir kullanıcı denetimi bir veya daha fazla nesne durumu devam ettirmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="9d3d4-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="9d3d4-107">İlk olarak, bir kullanıcı denetimi bir arama için filtre ölçütlerini belirtmek bir kullanıcı için özelliğini temsil eden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9d3d4-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="9d3d4-108">Ayrıca, bir yardımcı filtre sınıfı filtresi bilgileri depolamak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9d3d4-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="9d3d4-109">Birkaç kullanıcı arabirimi öğeleri bazı yöntemleri ve özellikleri filtre sınıf örneği geçerli filtre bilgilerini depolamak için birlikte filtre denetimi eklenir.</span><span class="sxs-lookup"><span data-stu-id="9d3d4-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="9d3d4-110">Ardından, kullanıcı denetimi kalıcılığı RegisterRequiresControlState yöntemi ve ilişkili kaydetme/geri yükleme yöntemleri kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9d3d4-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="9d3d4-111">Bu yöntemler filtre sınıf ve kendi veri örneği sayfasına geri göndermeler sırasında depolar.</span><span class="sxs-lookup"><span data-stu-id="9d3d4-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="9d3d4-112">Son olarak, birden çok nesne denetim durumu uygulamasında depolamak nasıl tartışması yoktur.</span><span class="sxs-lookup"><span data-stu-id="9d3d4-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="9d3d4-113">&#9654; (23 dakika) videoyu izleyin</span><span class="sxs-lookup"><span data-stu-id="9d3d4-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
