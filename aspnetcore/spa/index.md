---
title: ASP.NET Core ile tek sayfa uygulaması şablonlarını kullanma
author: SteveSandersonMS
description: Yükleme ve ASP.NET Core tek sayfa uygulama (SPA) proje şablonları kullanmaya başlama hakkında bilgi edinin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="f771f-103">ASP.NET Core ile tek sayfa uygulaması şablonlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="f771f-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="f771f-104">Yayımlanan .NET Core SDK Angular için tepki, eski proje şablonları içerir ve Redux ile tepki 2.0.x.</span><span class="sxs-lookup"><span data-stu-id="f771f-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="f771f-105">Bu belge, bu eski proje şablonları hakkında değil.</span><span class="sxs-lookup"><span data-stu-id="f771f-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="f771f-106">Bu belge, en son Angular için tepki, olduğundan ve ASP.NET Core 2.0 el ile yüklenebilir Redux şablonları ile tepki.</span><span class="sxs-lookup"><span data-stu-id="f771f-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="f771f-107">Şablonlar, ASP.NET Core 2.1 ile birlikte varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="f771f-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="f771f-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f771f-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="f771f-109">[Node.js](https://nodejs.org), 6 veya sonraki sürümü</span><span class="sxs-lookup"><span data-stu-id="f771f-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="f771f-110">Yükleme</span><span class="sxs-lookup"><span data-stu-id="f771f-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f771f-111">Şablonları zaten ASP.NET Core 2.1 ile birlikte yüklenir.</span><span class="sxs-lookup"><span data-stu-id="f771f-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f771f-112">ASP.NET Core 2.0 Angular için güncelleştirilmiş ASP.NET Core şablonları yüklemek için aşağıdaki komutu çalıştırın, varsa, tepki ve Redux ile tepki:</span><span class="sxs-lookup"><span data-stu-id="f771f-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="f771f-113">Şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="f771f-113">Use the templates</span></span>

* [<span data-ttu-id="f771f-114">Açısal proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="f771f-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="f771f-115">Tepki proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="f771f-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="f771f-116">Tepki Redux proje şablonu ile kullanma</span><span class="sxs-lookup"><span data-stu-id="f771f-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
