---
title: ASP.NET Core LibMan ile istemci tarafı kitaplık edinme
author: scottaddie
description: Kitaplık Yöneticisi'ni (LibMan) kullanarak bir ASP.NET Core projesi içinde istemci tarafı kitaplık varlıkları yüklemeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: b21ab0dbeda043dceda5376bcd95ccd334707c5a
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910011"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a>ASP.NET Core LibMan ile istemci tarafı kitaplık edinme

Tarafından [Scott Addie](https://twitter.com/Scott_Addie)

Kitaplık Yöneticisi'ni (LibMan), basit ve istemci tarafı kitaplık edinme aracıdır. LibMan, popüler kitaplıklar ve çerçeveler dosya sisteminden veya sitesinden indirir bir [content delivery Network'te (CDN)](https://wikipedia.org/wiki/Content_delivery_network). Desteklenen CDN'ler dahil [CDNJS](https://cdnjs.com/) ve [unpkg](https://unpkg.com/#/). Seçilen kitaplık dosyaları getirildi ve ASP.NET Core projesi içinde uygun konuma yerleştirilir.

## <a name="libman-use-cases"></a>LibMan kullanım örnekleri

LibMan aşağıdaki avantajları sunar:

* Yalnızca gereksinim duyduğunuz kitaplık dosyaları indirilir.
* Ek, gibi araçları [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), ve [Web](https://webpack.js.org), kitaplıktaki dosyaların bir alt kümesine almak gerekli değildir.
* Derleme görevleri veya el ile dosya kopyalama başvurmadan dosyaları belirli bir konumda yerleştirilebilir.

LibMan'ın avantajları hakkında daha fazla bilgi için izleme [Modern ön uç web geliştirme Visual Studio 2017'de: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).

LibMan bir paket yönetim sistemi değildir. Npm gibi bir paket Yöneticisi zaten kullanıyorsanız veya [yarn](https://yarnpkg.com), bunu yapmaya devam edin. LibMan araçlarda değiştirilecek gelişmemişti.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:client-side/libman/libman-vs>
* [LibMan GitHub deposu](https://github.com/aspnet/LibraryManager)
