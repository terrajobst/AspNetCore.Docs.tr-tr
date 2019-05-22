---
title: ASP.NET Core Blazor desteklenen platformlar
author: guardrex
description: Desteklenen platformlar ile ilgili bilgi edinmek için ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/21/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 12ef3885044cfca17c2e7ffdb248fdebef26c48a
ms.sourcegitcommit: e67356f5e643a5d43f6d567c5c998ce6002bdeb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005347"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core Blazor desteklenen platformlar

Tarafından [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Tarayıcı gereksinimleri

### <a name="blazor-client-side"></a>Blazor istemci tarafı

| Tarayıcı                          | Sürüm               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Geçerli               |
| Mozilla Firefox                  | Geçerli               |
| Google Chrome, Android dahil olmak üzere | Geçerli               |
| Safari iOS dahil,            | Geçerli               |
| Microsoft Internet Explorer      | Desteklenmiyor&dagger; |

&dagger;Microsoft Internet Explorer desteklemiyor [WebAssembly](http://webassembly.org).

İstemci tarafı Blazor barındırma modeli hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models#client-side>.

### <a name="blazor-server-side"></a>Blazor sunucu tarafı

| Tarayıcı                          | Sürüm    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Geçerli    |
| Mozilla Firefox                  | Geçerli    |
| Google Chrome, Android dahil olmak üzere | Geçerli    |
| Safari iOS dahil,            | Geçerli    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Ek polyfills gereklidir (örneğin, öneriler aracılığıyla eklenebilir bir [Polyfill.io](https://polyfill.io/v3/) paket).

Sunucu tarafı Blazor barındırma modeli hakkında daha fazla bilgi için bkz. <xref:blazor/hosting-models#server-side>.
