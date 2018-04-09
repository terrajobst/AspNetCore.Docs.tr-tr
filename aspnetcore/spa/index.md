---
title: ASP.NET Core ile tek sayfa uygulaması şablonlarını kullanma
author: SteveSandersonMS
description: Yükleme ve ASP.NET Core tek sayfa uygulama (SPA) proje şablonları kullanmaya başlama hakkında bilgi edinin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="1a1a1-103">ASP.NET Core ile tek sayfa uygulaması şablonlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="1a1a1-103">Use the Single Page Application templates with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="1a1a1-104">Yayımlanan .NET Core SDK Angular için tepki, eski proje şablonları içerir ve Redux ile tepki 2.0.x.</span><span class="sxs-lookup"><span data-stu-id="1a1a1-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="1a1a1-105">Bu belge, bu eski proje şablonları hakkında değil.</span><span class="sxs-lookup"><span data-stu-id="1a1a1-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="1a1a1-106">Bu belge, en son Angular için tepki, olduğundan ve ASP.NET Core 2.0 el ile yüklenebilir Redux şablonları ile tepki.</span><span class="sxs-lookup"><span data-stu-id="1a1a1-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="1a1a1-107">Şablonlar, ASP.NET Core 2.1 ile birlikte varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="1a1a1-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a1a1-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="1a1a1-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="1a1a1-109">[Node.js](https://nodejs.org), 6 veya sonraki sürümü</span><span class="sxs-lookup"><span data-stu-id="1a1a1-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="1a1a1-110">Yükleme</span><span class="sxs-lookup"><span data-stu-id="1a1a1-110">Installation</span></span>

<span data-ttu-id="1a1a1-111">ASP.NET Core 2.0 Angular için güncelleştirilmiş ASP.NET Core şablonları yüklemek için aşağıdaki komutu çalıştırın, varsa, tepki ve Redux ile tepki:</span><span class="sxs-lookup"><span data-stu-id="1a1a1-111">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="1a1a1-112">Şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="1a1a1-112">Use the templates</span></span>

- [<span data-ttu-id="1a1a1-113">Açısal proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="1a1a1-113">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="1a1a1-114">Tepki proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="1a1a1-114">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="1a1a1-115">Tepki Redux proje şablonu ile kullanma</span><span class="sxs-lookup"><span data-stu-id="1a1a1-115">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
