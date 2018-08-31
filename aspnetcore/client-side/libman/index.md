---
title: ASP.NET Core LibMan ile istemci tarafı kitaplık edinme
author: scottaddie
description: Kitaplık Yöneticisi'ni (LibMan) kullanarak bir ASP.NET Core projesi içinde istemci tarafı kitaplık varlıkları yüklemeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312365"
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
* <xref:client-side/libman/libman-cli>
* [LibMan GitHub deposu](https://github.com/aspnet/LibraryManager)
