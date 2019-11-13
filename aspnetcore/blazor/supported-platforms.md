---
title: Desteklenen Blazor platformları ASP.NET Core
author: guardrex
description: ASP.NET Core Blazoriçin desteklenen platformlar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/supported-platforms
ms.openlocfilehash: de51296cc8785474e1c1406cfd5d4e5bd4050172
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962739"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>Desteklenen Blazor platformları ASP.NET Core

[Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Tarayıcı gereksinimleri

### <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

| Tarayıcı                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Geçerli               |
| Mozilla Firefox                  | Geçerli               |
| Android dahil Google Chrome | Geçerli               |
| İOS dahil Safari            | Geçerli               |
| Microsoft Internet Explorer      | Desteklenmez&dagger; |

&dagger;Microsoft Internet Explorer [Webassembly](https://webassembly.org)'yi desteklemez.

### <a name="opno-locblazor-server"></a>Blazor sunucusu

| Tarayıcı                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Geçerli    |
| Mozilla Firefox                  | Geçerli    |
| Android dahil Google Chrome | Geçerli    |
| İOS dahil Safari            | Geçerli    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Ek polydolgular gereklidir (örneğin, [Polyfill.io](https://polyfill.io/v3/) bir paket aracılığıyla taahhüt eklenebilir).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:blazor/hosting-models>
