---
title: ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Azure'da barındırılan bir ASP.NET Core uygulaması için bir DevOps işlem hattı oluşturmaya uçtan uca yönergeler sağlar. bir kılavuz.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722731"
---
# <a name="devops-with-aspnet-core-and-azure"></a>ASP.NET Core ve Azure ile DevOps

.NET için Azure geliştirme yaşam döngüsü kılavuzuna Hoş Geldiniz! Bu kılavuzda, .NET araçları ve işlemleri kullanarak Azure'da geçici bir geliştirme yaşam döngüsü oluşturmanın temel kavramları tanıtır. Bu kılavuzu tamamladıktan sonra olgun bir DevOps araç zincirinde avantajlarını yararlanabileceksiniz.

## <a name="who-this-guide-is-for"></a>Bu kılavuz için olan

Deneyimli bir ASP.NET geliştiricisi (200-300 düzeyi) olmalıdır. Biz, bu bölümde ele alacağız gibi Azure hakkında her şeyi bilmeniz gerekmez. Bu kılavuz, ayrıca geliştirme işlemleri daha fazla odaklanan DevOps mühendisleri için yararlı olabilir.

Bu kılavuz, Windows geliştiricileri hedefler. Ancak, Linux ve Macos'ta .NET Core tarafından tam olarak desteklenir. Linux/macOS farklar için çağrılar için bu kılavuzu Linux/macOS için uyarlamak üzere izleyin.

## <a name="what-this-guide-doesnt-cover"></a>Bu kılavuzda ele alınmamıştır

Bu kılavuzda, .NET geliştiricileri için bir uçtan uca sürekli dağıtım deneyimi üzerinde odaklanmıştır. Her şey Azure için ayrıntılı bir kılavuz değildir ve, kapsamlı bir şekilde .NET API'leri Azure Hizmetleri için odak değil. Vurgu tüm sürekli tümleştirme, dağıtım, izleme ve hata ayıklama ' dir. Kılavuzu sonuna, sonraki adımlar için öneriler sunulur. Öneri ASP.NET geliştiricileri için kullanışlı olan bir Azure platform hizmetlerini içerir.

## <a name="whats-in-this-guide"></a>Bu kılavuzda nedir

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Araçlar ve indirmeler](xref:azure/devops/tools-and-downloads)

Bu kılavuzda kullanılan araçları almayı öğrenin.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[App Service'e dağıtma](xref:azure/devops/deploy-to-app-service)

Azure App Service'e bir ASP.NET Core uygulaması dağıtmak için çeşitli yöntemler hakkında bilgi edinin.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Sürekli tümleştirme ve dağıtım](xref:azure/devops/cicd)

Bir uçtan uca sürekli tümleştirme ve dağıtım çözümü için GitHub, VSTS ve Azure ile ASP.NET Core uygulamanızı oluşturun.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[İzleme ve hata ayıklama](xref:azure/devops/monitor)

İzleme, sorun giderme ve uygulamanızı ayarlamak için Azure'nın araçlarını kullanın.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Sonraki adımlar](xref:azure/devops/next-steps)

Azure Öğrenme ASP.NET Core Geliştirici diğer öğrenme yolları.

## <a name="acknowledgments"></a>İlgili kaynaklar

Bu kılavuzda yararlı önerileriniz katkıda .NET topluluk herkese teşekkür ederiz! Özellikle bu yazıda son gözden geçirme için katkıda bulunan aşağıdaki topluluk üyeleri teşekkür istiyoruz:

* [SAM Wronski](https://www.youtube.com/c/worldofzerodevelopment)
* [Jeffrey Palermo](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a>Sonuç

Bu kılavuz, ASP.NET Core ve Azure App Service geçici olarak oluşturulmuş bir sürekli tümleştirme geliştirme yaşam döngüsü oluşturmak için hazırlar.

## <a name="additional-reading"></a>Ek okuma

* [Bulut bilgi işlem nedir?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Örnekler bulut bilgi işlem](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [Iaas nedir?](https://azure.microsoft.com/overview/what-is-iaas/)
* [PaaS nedir?](https://azure.microsoft.com/overview/what-is-paas/)
