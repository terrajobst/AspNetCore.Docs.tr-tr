---
title: Desteklenen Blazor platformları ASP.NET Core
author: guardrex
description: ASP.NET Core Blazoriçin desteklenen platformlar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658860"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core Blazor desteklenen platformlar

[Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Tarayıcı gereksinimleri

### <a name="blazor-webassembly"></a>Blazor WebAssembly

| Tarayıcı                          | Sürüm               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Geçerli               |
| Mozilla Firefox                  | Geçerli               |
| Android dahil Google Chrome | Geçerli               |
| İOS dahil Safari            | Geçerli               |
| Microsoft Internet Explorer      | Desteklenmez&dagger; |

&dagger;Microsoft Internet Explorer, [Webassembly](https://webassembly.org)'yi desteklemez.

### <a name="blazor-server"></a>Blazor Server

| Tarayıcı                          | Sürüm    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Geçerli    |
| Mozilla Firefox                  | Geçerli    |
| Android dahil Google Chrome | Geçerli    |
| İOS dahil Safari            | Geçerli    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;ek polydolgular gereklidir (örneğin, [Polyfill.io](https://polyfill.io/v3/) bir paket aracılığıyla taahhüt eklenebilir).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:blazor/hosting-models>
