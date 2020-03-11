---
title: ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
ms.custom: mvc, seodec18
uid: azure/devops/index
ms.openlocfilehash: f45bb2a5dd4b3d1a820085ede7ce3219045ed80b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658083"
---
# <a name="devops-with-aspnet-core-and-azure"></a>ASP.NET Core ve Azure ile DevOps

[![kapak resmi](./media/cover-large.png)](https://aka.ms/devopsbook)

By [Cam Soper](https://twitter.com/camsoper) ve [Scott Ade](https://twitter.com/scottaddie)

Bu kılavuz, [indirilebilir BIR PDF e-kitabı](https://aka.ms/devopsbook)olarak sunulmaktadır.

## <a name="welcome"></a>Hoş Geldiniz 

.NET için Azure geliştirme yaşam döngüsü kılavuzuna Hoş Geldiniz! Bu kılavuzda, .NET araçları ve işlemleri kullanarak Azure'da geçici bir geliştirme yaşam döngüsü oluşturmanın temel kavramları tanıtır. Bu kılavuzu tamamladıktan sonra olgun bir DevOps araç zincirinde avantajlarını yararlanabileceksiniz.

## <a name="who-this-guide-is-for"></a>Bu kılavuz için olan

Deneyimli bir ASP.NET Core geliştirici (200-300 düzeyi) olmalıdır. Biz, bu bölümde ele alacağız gibi Azure hakkında her şeyi bilmeniz gerekmez. Bu kılavuz, ayrıca geliştirme işlemleri daha fazla odaklanan DevOps mühendisleri için yararlı olabilir.

Bu kılavuz, Windows geliştiricileri hedefler. Ancak, Linux ve Macos'ta .NET Core tarafından tam olarak desteklenir. Linux/macOS farklar için çağrılar için bu kılavuzu Linux/macOS için uyarlamak üzere izleyin.

## <a name="what-this-guide-doesnt-cover"></a>Bu kılavuzda ele alınmamıştır

Bu kılavuzda, .NET geliştiricileri için bir uçtan uca sürekli dağıtım deneyimi üzerinde odaklanmıştır. Her şey Azure için ayrıntılı bir kılavuz değildir ve, kapsamlı bir şekilde .NET API'leri Azure Hizmetleri için odak değil. Vurgu tüm sürekli tümleştirme, dağıtım, izleme ve hata ayıklama ' dir. Kılavuzu sonuna, sonraki adımlar için öneriler sunulur. Öneri, ASP.NET Core geliştiricileri için yararlı olan bir Azure platform hizmetlerini içerir.

## <a name="whats-in-this-guide"></a>Bu kılavuzda nedir

### <a name="tools-and-downloads"></a>[Araçlar ve indirmeler](xref:azure/devops/tools-and-downloads)

Bu kılavuzda kullanılan araçları almayı öğrenin.

### <a name="deploy-to-app-service"></a>[App Service’e dağıtma](xref:azure/devops/deploy-to-app-service)

Azure App Service'e bir ASP.NET Core uygulaması dağıtmak için çeşitli yöntemler hakkında bilgi edinin.

### <a name="continuous-integration-and-deployment"></a>[Sürekli tümleştirme ve dağıtım](xref:azure/devops/cicd)

Bir uçtan uca sürekli tümleştirme ve dağıtım çözümü için GitHub, Azure DevOps Services ve Azure ile ASP.NET Core uygulamanızı oluşturun.

### <a name="monitor-and-debug"></a>[İzleme ve hata ayıklama](xref:azure/devops/monitor)

İzleme, sorun giderme ve uygulamanızı ayarlamak için Azure'nın araçlarını kullanın.

### <a name="next-steps"></a>[Sonraki adımlar](xref:azure/devops/next-steps)

Azure Öğrenme ASP.NET Core Geliştirici diğer öğrenme yolları.

## <a name="additional-introductory-reading"></a>Ek giriş okuma

Bu bulut için ilk maruz kalma riskinizi ise, bu makaleler temellerini açıklar.

* [Bulut bilgi Işlem nedir?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Bulut bilgi Işlem örnekleri](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [IaaS Nedir?](https://azure.microsoft.com/overview/what-is-iaas/)
* [PaaS nedir?](https://azure.microsoft.com/overview/what-is-paas/)
