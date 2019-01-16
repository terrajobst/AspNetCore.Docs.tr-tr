---
title: ASP.NET Core ile React-ile-Redux proje şablonu kullanın
author: SteveSandersonMS
description: React ile Redux ve oluşturma-react-uygulama için ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341627"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="91867-103">ASP.NET Core ile React-ile-Redux proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="91867-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="91867-104">Bu belge ASP.NET Core 2.0 sürümünde bulunan React-ile-Redux proje şablonu ait değil.</span><span class="sxs-lookup"><span data-stu-id="91867-104">This documentation doesn't pertain to the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="91867-105">Bu, el ile güncelleştirebilirsiniz yeni React-ile-Redux şablonu için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="91867-105">It pertains to the newer React-with-Redux template that you can update manually.</span></span> <span data-ttu-id="91867-106">ASP.NET Core 2.1 veya daha sonraki şablonudur.</span><span class="sxs-lookup"><span data-stu-id="91867-106">The template is available in ASP.NET Core 2.1 or later.</span></span>

::: moniker-end

<span data-ttu-id="91867-107">Güncelleştirilmiş Redux ile React proje şablonu kullanarak ASP.NET Core uygulamaları, React Redux, uygun bir başlama noktası sağlar ve [oluşturma-react-app](https://github.com/facebookincubator/create-react-app) (CRA) kuralları, istemci tarafı zengin kullanıcı arabirimi (UI) uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="91867-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="91867-108">Proje oluşturma komutuna dışında React-ile-Redux şablonu hakkındaki tüm bilgileri React şablonu ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="91867-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="91867-109">Bu proje türü oluşturmak için çalıştırılması `dotnet new reactredux` yerine `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="91867-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="91867-110">Her iki React tabanlı şablonlar için ortak olan işlevselliği hakkında daha fazla bilgi için bkz: [React şablon belgeleri](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="91867-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>

<span data-ttu-id="91867-111">IIS React-ile-Redux bir alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: [açarken kilitlenmesi şablon 2.1: IIS üzerinde SPA kullanılamıyor (aspnet/şablon &num;555)](https://github.com/aspnet/Templating/issues/555).</span><span class="sxs-lookup"><span data-stu-id="91867-111">For information on configuring a React-with-Redux sub-application in IIS, see [ReactRedux Template 2.1: Unable to use SPA on IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span></span>
