---
title: ASP.NET Core ile tepki-ile-Redux proje şablonu kullanın
author: SteveSandersonMS
description: ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile tepki Redux ve oluşturma tepki-uygulama için başlama öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 7ec4f6d53a4723ace087b1dc256de7845cb44cc6
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2018
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="71483-103">ASP.NET Core ile tepki-ile-Redux proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="71483-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="71483-104">Bu belge hakkında tepki-ile-Redux proje şablonu ASP.NET Core 2. 0 ' dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="71483-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="71483-105">Bunu el ile güncelleştirme yapabilmeniz için daha yeni tepki-ile-Redux hakkında şablonudur.</span><span class="sxs-lookup"><span data-stu-id="71483-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="71483-106">Şablon ASP.NET Core 2.1 içinde varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="71483-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="71483-107">Güncelleştirilmiş tepki-ile-Redux proje şablonu kullanarak ASP.NET Core uygulamaları tepki Redux, uygun bir başlama noktası sağlar ve [oluşturma tepki-uygulama](https://github.com/facebookincubator/create-react-app) (CRA) kuralları zengin, istemci tarafı kullanıcı arabirimini (UI).</span><span class="sxs-lookup"><span data-stu-id="71483-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="71483-108">Proje oluşturma komutu dışında tepki-ile-Redux şablonu hakkındaki tüm bilgileri tepki şablonu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="71483-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="71483-109">Bu proje türü oluşturmak için çalıştırın `dotnet new reactredux` yerine `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="71483-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="71483-110">Her iki tepki tabanlı şablonlar için ortak işlevselliği hakkında daha fazla bilgi için bkz: [tepki şablon belgelerine](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="71483-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
