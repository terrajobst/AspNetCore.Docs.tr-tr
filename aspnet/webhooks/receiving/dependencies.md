---
uid: webhooks/receiving/dependencies
title: ASP.NET Web Kancalarını alıcı bağımlılıkları | Microsoft Docs
author: rick-anderson
description: Alıcı bağımlılıklar ve ASP.NET Web Kancalarını bağımlılık ekleme.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573279"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="c6da7-103">ASP.NET Web Kancalarını alıcı bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="c6da7-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="c6da7-104">Microsoft ASP.NET WebHooks bağımlılık ekleme aklınızda tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6da7-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="c6da7-105">Çoğu bağımlılıkları sisteminde bir bağımlılık ekleme altyapısını kullanarak alternatif uygulamaları ile değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c6da7-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="c6da7-106">Lütfen bakın [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) alıcı bağımlılıkları listesi.</span><span class="sxs-lookup"><span data-stu-id="c6da7-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="c6da7-107">Hiçbir bağımlılık kaydolduysanız varsayılan uygulama kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6da7-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="c6da7-108">Lütfen bakın [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) varsayılan uygulamaları listesi.</span><span class="sxs-lookup"><span data-stu-id="c6da7-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
