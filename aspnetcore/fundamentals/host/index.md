---
title: Web ana bilgisayarı ve ASP.NET Core genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu ASP.NET Core Web ana bilgisayarı ve .NET genel ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc,seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 3e67d8338aa7ac1b1530d0498ee0126d36a8d72b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121524"
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
