---
title: ASP.NET core'da konak
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu ASP.NET Core Web ana bilgisayarı ve .NET genel ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336056"
---
# <a name="host-in-aspnet-core"></a>ASP.NET core'da konak

.NET uygulamaları ve başlatma yapılandırma bir *konak*. Uygulama başlatma ve ömür yönetimi için konak sorumludur. İki ana API'leri kullanılabilir duruma gelir:

* [Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.
* [Genel konak](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya üzeri) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalıştırma uygulamaları) barındırmak için uygundur. Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere herhangi bir türden barındırma için uygun olacaktır. Genel konak Web ana bilgisayarı sonunda yerini alır.

ASP.NET Core barındırma *web uygulamaları*, geliştiriciler, temel Web barındırma kullanmalıdır <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Barındırma için *olmayan web apps*, geliştiricilere genel bir temel konak kullanması gereken <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.

<xref:fundamentals/configuration/platform-specific-configuration>  
ASP.NET Core uygulaması kullanarak başvurulan veya başvurulmayan bir bütünleştirilmiş koddan geliştirmek nasıl bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulaması.
