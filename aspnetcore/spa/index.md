---
title: "Tek sayfa uygulaması şablonları kullanın."
author: SteveSandersonMS
description: "Yükleme ve ASP.NET Core tek sayfa uygulama (SPA) proje şablonları kullanmaya başlama hakkında bilgi edinin."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 63b56de101199e9ea0d66d89d2dd7288e47902f6
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-single-page-application-templates"></a><span data-ttu-id="8cb20-103">Tek sayfa uygulaması şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="8cb20-103">Use the Single Page Application templates</span></span>

> [!NOTE]
> <span data-ttu-id="8cb20-104">Yayımlanan .NET Core SDK Angular için tepki, eski proje şablonları içerir ve Redux ile tepki 2.0.x.</span><span class="sxs-lookup"><span data-stu-id="8cb20-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="8cb20-105">Bu belge, bu eski proje şablonları hakkında değil.</span><span class="sxs-lookup"><span data-stu-id="8cb20-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="8cb20-106">Bu belge, en son Angular için tepki, olduğundan ve ASP.NET Core 2.0 el ile yüklenebilir Redux şablonları ile tepki.</span><span class="sxs-lookup"><span data-stu-id="8cb20-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="8cb20-107">Şablonlar, ASP.NET Core 2.1 ile birlikte varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="8cb20-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8cb20-108">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8cb20-108">Prerequisites</span></span>

* <span data-ttu-id="8cb20-109">[.NET core SDK](https://www.microsoft.com/net/download), sürüm 2.0.0 veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="8cb20-109">[.NET Core SDK](https://www.microsoft.com/net/download), version 2.0.0 or later</span></span>
* <span data-ttu-id="8cb20-110">[Node.js](https://nodejs.org), 6 veya sonraki sürümü</span><span class="sxs-lookup"><span data-stu-id="8cb20-110">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="8cb20-111">Yükleme</span><span class="sxs-lookup"><span data-stu-id="8cb20-111">Installation</span></span>

<span data-ttu-id="8cb20-112">ASP.NET Core 2.0 Angular için güncelleştirilmiş ASP.NET Core şablonları yüklemek için aşağıdaki komutu çalıştırın, varsa, tepki ve Redux ile tepki:</span><span class="sxs-lookup"><span data-stu-id="8cb20-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="8cb20-113">Şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="8cb20-113">Use the templates</span></span>

- [<span data-ttu-id="8cb20-114">Açısal proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="8cb20-114">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="8cb20-115">Tepki proje şablonu kullanın</span><span class="sxs-lookup"><span data-stu-id="8cb20-115">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="8cb20-116">Tepki Redux proje şablonu ile kullanma</span><span class="sxs-lookup"><span data-stu-id="8cb20-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
