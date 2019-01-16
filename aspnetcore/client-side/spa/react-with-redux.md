---
title: ASP.NET Core ile React-ile-Redux proje şablonu kullanın
author: SteveSandersonMS
description: React ile Redux ve oluşturma-react-uygulama için ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341627"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>ASP.NET Core ile React-ile-Redux proje şablonu kullanın

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Bu belge ASP.NET Core 2.0 sürümünde bulunan React-ile-Redux proje şablonu ait değil. Bu, el ile güncelleştirebilirsiniz yeni React-ile-Redux şablonu için geçerlidir. ASP.NET Core 2.1 veya daha sonraki şablonudur.

::: moniker-end

Güncelleştirilmiş Redux ile React proje şablonu kullanarak ASP.NET Core uygulamaları, React Redux, uygun bir başlama noktası sağlar ve [oluşturma-react-app](https://github.com/facebookincubator/create-react-app) (CRA) kuralları, istemci tarafı zengin kullanıcı arabirimi (UI) uygulamak için.

Proje oluşturma komutuna dışında React-ile-Redux şablonu hakkındaki tüm bilgileri React şablonu ile aynıdır. Bu proje türü oluşturmak için çalıştırılması `dotnet new reactredux` yerine `dotnet new react`. Her iki React tabanlı şablonlar için ortak olan işlevselliği hakkında daha fazla bilgi için bkz: [React şablon belgeleri](xref:spa/react).

IIS React-ile-Redux bir alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: [açarken kilitlenmesi şablon 2.1: IIS üzerinde SPA kullanılamıyor (aspnet/şablon &num;555)](https://github.com/aspnet/Templating/issues/555).
