---
title: Visual Studio ile Windows ASP.NET Core MVC ile bir web uygulaması oluşturma
author: rick-anderson
description: ASP.NET Core Windows Visual Studio kullanarak MVC giriş İçindekiler bakın.
manager: wpickett
ms.author: riande
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/index
ms.openlocfilehash: 84ccaee9bc9e6af0b420de9a35caa36f49d8f6bb
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729814"
---
# <a name="create-a-web-app-with-aspnet-core-mvc-on-windows-with-visual-studio"></a><span data-ttu-id="e4623-103">Visual Studio ile Windows ASP.NET Core MVC ile bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4623-103">Create a web app with ASP.NET Core MVC on Windows with Visual Studio</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="e4623-104">Bu öğretici 3 sürümü vardır:</span><span class="sxs-lookup"><span data-stu-id="e4623-104">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e4623-105">Windows: Bu serisi</span><span class="sxs-lookup"><span data-stu-id="e4623-105">Windows: This series</span></span>
* <span data-ttu-id="e4623-106">macOS: [Mac için Visual Studio ile ASP.NET Core MVC uygulama oluşturma](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e4623-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e4623-107">macOS, Linux ve Windows: [Visual Studio Code ile bir ASP.NET Core MVC uygulaması oluşturma](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e4623-107">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="e4623-108">Eğitmen serisi aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="e4623-108">The tutorial series includes the following:</span></span>

1. [<span data-ttu-id="e4623-109">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e4623-109">Get started</span></span>](start-mvc.md)
1. [<span data-ttu-id="e4623-110">Denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="e4623-110">Add a controller</span></span>](adding-controller.md)
1. [<span data-ttu-id="e4623-111">Görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="e4623-111">Add a view</span></span>](adding-view.md)
1. [<span data-ttu-id="e4623-112">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="e4623-112">Add a model</span></span>](adding-model.md)
1. [<span data-ttu-id="e4623-113">SQL Server LocalDB ile çalışma</span><span class="sxs-lookup"><span data-stu-id="e4623-113">Work with SQL Server LocalDB</span></span>](working-with-sql.md)
1. [<span data-ttu-id="e4623-114">Denetleyici metotları ve görünümleri</span><span class="sxs-lookup"><span data-stu-id="e4623-114">Controller methods and views</span></span>](controller-methods-views.md)
1. [<span data-ttu-id="e4623-115">Arama ekleme</span><span class="sxs-lookup"><span data-stu-id="e4623-115">Add search</span></span>](search.md)
1. [<span data-ttu-id="e4623-116">Yeni alan ekleme</span><span class="sxs-lookup"><span data-stu-id="e4623-116">Add a new field</span></span>](new-field.md)
1. [<span data-ttu-id="e4623-117">Doğrulama ekleme</span><span class="sxs-lookup"><span data-stu-id="e4623-117">Add validation</span></span>](validation.md)
1. [<span data-ttu-id="e4623-118">Details ve Delete metotlarını inceleme</span><span class="sxs-lookup"><span data-stu-id="e4623-118">Examine the Details and Delete methods</span></span>](details.md)