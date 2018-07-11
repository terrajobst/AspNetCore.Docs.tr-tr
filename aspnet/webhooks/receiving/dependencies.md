---
uid: webhooks/receiving/dependencies
title: ASP.NET Web kancaları alıcı bağımlılıkları | Microsoft Docs
author: rick-anderson
description: Alıcı bağımlılıkları ve ASP.NET Web kancaları bağımlılık ekleme.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817843"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="f323b-103">ASP.NET Web kancaları alıcı bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="f323b-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="f323b-104">Microsoft ASP.NET WebHooks bağımlılık ekleme düşünülerek tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="f323b-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="f323b-105">Çoğu bağımlılıkları sisteminde diğer uygulamaları ile bir bağımlılık ekleme altyapısı kullanılarak değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f323b-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="f323b-106">Lütfen [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) alıcı bağımlılıkları listesi.</span><span class="sxs-lookup"><span data-stu-id="f323b-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="f323b-107">Hiçbir bağımlılık kayıtlı ise varsayılan bir uygulama kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f323b-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="f323b-108">Lütfen [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) varsayılan uygulamaları listesi.</span><span class="sxs-lookup"><span data-stu-id="f323b-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
