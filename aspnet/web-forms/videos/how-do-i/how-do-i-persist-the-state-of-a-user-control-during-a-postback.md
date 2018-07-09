---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: Bu video Chris piksel içinde bir kullanıcı denetimi içinde bir veya daha fazla nesne durumunu sürdürülmesi gösterilmektedir. İlk olarak, bir kullanıcı denetimi abilit temsil eden oluşturuldu...
ms.author: aspnetcontent
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 7862d83d3df3ca5407b7d8fd465cf42da8e7228a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805184"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Nasıl yapılır]: geri gönderme sırasında bir kullanıcı denetiminin durumunu kalıcı
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="e4880-104">tarafından [Chris piksel](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="e4880-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="e4880-105">Bu video Chris piksel içinde bir kullanıcı denetimi içinde bir veya daha fazla nesne durumunu sürdürülmesi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e4880-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="e4880-106">İlk olarak, bir kullanıcı denetimi için arama filtre ölçütlerini belirtmek bir kullanıcı özelliği temsil eden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e4880-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="e4880-107">Ayrıca, filtre bilgileri depolamak için bir yardımcı filtre sınıfı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e4880-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="e4880-108">Birkaç kullanıcı arabirimi öğeleri yanı sıra bazı yöntemleri ve özellikleri filtre sınıfı örneğinde geçerli filtre bilgilerini depolamak için filtre denetimi eklenir.</span><span class="sxs-lookup"><span data-stu-id="e4880-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="e4880-109">Ardından, kullanıcı denetimi Kalıcılık RegisterRequiresControlState yöntemi ve ilişkili kaydedilemiyor/geri yöntemleri kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e4880-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="e4880-110">Bu yöntemler, sayfa geri gönderme sırasında filtre sınıf ve onun veri örneğini depolar.</span><span class="sxs-lookup"><span data-stu-id="e4880-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="e4880-111">Son olarak, ayrıntılı bir denetim durumu uygulamada birden fazla nesne depolamak nasıl yoktur.</span><span class="sxs-lookup"><span data-stu-id="e4880-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="e4880-112">&#9654;Videoyu (23 dakika)</span><span class="sxs-lookup"><span data-stu-id="e4880-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
