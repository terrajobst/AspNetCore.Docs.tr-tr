---
uid: webhooks/receiving/dependencies
title: ASP.NET Web kancaları alıcı bağımlılıkları | Microsoft Docs
author: rick-anderson
description: Alıcı bağımlılıkları ve ASP.NET Web kancaları bağımlılık ekleme.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401195"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>ASP.NET Web kancaları alıcı bağımlılıkları

Microsoft ASP.NET WebHooks bağımlılık ekleme düşünülerek tasarlanmıştır. Çoğu bağımlılıkları sisteminde diğer uygulamaları ile bir bağımlılık ekleme altyapısı kullanılarak değiştirilebilir.

Lütfen [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) alıcı bağımlılıkları listesi. Hiçbir bağımlılık kayıtlı ise varsayılan bir uygulama kullanılır. Lütfen [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) varsayılan uygulamaları listesi.
