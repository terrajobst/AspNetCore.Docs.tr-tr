---
title: Web ana bilgisayarı ve ASP.NET Core genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu ASP.NET Core Web ana bilgisayarı ve .NET genel ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284584"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>Web ana bilgisayarı ve ASP.NET Core genel ana bilgisayar

.NET uygulamaları ve başlatma yapılandırma bir *konak*. Uygulama başlatma ve ömür yönetimi için konak sorumludur. İki ana API'leri kullanılabilir duruma gelir:

* [Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.
* [Genel konak](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya üzeri) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalıştırma uygulamaları) barındırmak için uygundur. Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere herhangi bir türden barındırma için uygun olacaktır. Genel konak Web ana bilgisayarı sonunda yerini alır.

ASP.NET Core barındırma *web uygulamaları*, geliştiriciler, temel Web barındırma kullanmalıdır <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Barındırma için *olmayan web apps*, geliştiricilere genel bir temel konak kullanması gereken <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.

<xref:fundamentals/configuration/platform-specific-configuration>  
ASP.NET Core uygulaması kullanarak başvurulan veya başvurulmayan bir bütünleştirilmiş koddan geliştirmek nasıl bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulaması.
