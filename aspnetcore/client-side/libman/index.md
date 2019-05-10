---
title: ASP.NET Core LibMan ile istemci tarafı kitaplık edinme
author: scottaddie
description: Kitaplık Yöneticisi'ni (LibMan) kullanarak bir ASP.NET Core projesi içinde istemci tarafı kitaplık varlıkları yüklemeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900152"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="f017e-103">ASP.NET Core LibMan ile istemci tarafı kitaplık edinme</span><span class="sxs-lookup"><span data-stu-id="f017e-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="f017e-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f017e-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f017e-105">Kitaplık Yöneticisi'ni (LibMan), basit ve istemci tarafı kitaplık edinme aracıdır.</span><span class="sxs-lookup"><span data-stu-id="f017e-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="f017e-106">LibMan, popüler kitaplıklar ve çerçeveler dosya sisteminden veya sitesinden indirir bir [content delivery Network'te (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span><span class="sxs-lookup"><span data-stu-id="f017e-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="f017e-107">Desteklenen CDN'ler dahil [CDNJS](https://cdnjs.com/) ve [unpkg](https://unpkg.com/#/).</span><span class="sxs-lookup"><span data-stu-id="f017e-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="f017e-108">Seçilen kitaplık dosyaları getirildi ve ASP.NET Core projesi içinde uygun konuma yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f017e-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="f017e-109">LibMan kullanım örnekleri</span><span class="sxs-lookup"><span data-stu-id="f017e-109">LibMan use cases</span></span>

<span data-ttu-id="f017e-110">LibMan aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="f017e-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="f017e-111">Yalnızca gereksinim duyduğunuz kitaplık dosyaları indirilir.</span><span class="sxs-lookup"><span data-stu-id="f017e-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="f017e-112">Ek, gibi araçları [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), ve [Web](https://webpack.js.org), kitaplıktaki dosyaların bir alt kümesine almak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f017e-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="f017e-113">Derleme görevleri veya el ile dosya kopyalama başvurmadan dosyaları belirli bir konumda yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f017e-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="f017e-114">LibMan'ın avantajları hakkında daha fazla bilgi için izleme [Visual Studio 2017'deki Modern ön uç web geliştirme: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span><span class="sxs-lookup"><span data-stu-id="f017e-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="f017e-115">LibMan bir paket yönetim sistemi değildir.</span><span class="sxs-lookup"><span data-stu-id="f017e-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="f017e-116">Npm gibi bir paket Yöneticisi zaten kullanıyorsanız veya [yarn](https://yarnpkg.com), bunu yapmaya devam edin.</span><span class="sxs-lookup"><span data-stu-id="f017e-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="f017e-117">LibMan araçlarda değiştirilecek gelişmemişti.</span><span class="sxs-lookup"><span data-stu-id="f017e-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f017e-118">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f017e-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="f017e-119">LibMan GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="f017e-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
