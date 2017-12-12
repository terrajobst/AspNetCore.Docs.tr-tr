---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: "Visual Studio 2013'te ASP.NET İskele | Microsoft Docs"
author: tfitzmac
description: "ASP.NET İskele Visual Studio 2013'te bulunan yeni bir özelliktir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Visual Studio 2013'te ASP.NET İskele
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET İskele Visual Studio 2013'te bulunan yeni bir özelliktir.


## <a name="overview"></a>Genel Bakış

ASP.NET İskele bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Visual Studio 2013 MVC ve Web API projeleri için önceden yüklenmiş kod oluşturucuları içerir. Hızlı veri modelleri ile etkileşime giren kodu eklemek istediğinizde yapı iskelesi projenize ekleyin. Yapı iskelesi kullanarak standart veri işlemleri projenizdeki geliştirmek için süreyi azaltabilir.

Varsayılan olarak, Visual Studio 2013 için bir Web Forms projesi oluşturma kodunu desteklemez ancak yapı iskelesi projeye MVC bağımlılıkları ekleme veya uzantı yükleme Web Forms ile kullanabilirsiniz. Her iki yaklaşımın aşağıda verilmiştir.

Visual Studio 2013 güncelleştirme 2 (şu anda RC), ASP.NET, senaryonuzun gereksinimlerini karşılamak için yapı İskelesi genişletme olanağı sağlar. Bu işlev ile bir özelleştirilmiş yapı iskelesi şablonu oluşturun ve yeni İskele Ekle iletişim kutusuna ekleyin. Özelleştirilmiş şablonda iskele kurulmuş öğe zaman oluşturulan kodu belirtin. Daha fazla bilgi için bkz: [Visual Studio için özel iskele kurucu oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Önkoşullar

ASP.NET İskele kullanmak için olması gerekir:

- Microsoft Visual Studio 2013
- Web geliştirici araçları (varsayılan Visual Studio 2013 yüklemesinin parçası)
- ASP.NET Web çerçeveleri ve araçları 2013 (varsayılan Visual Studio 2013 yüklemesinin parçası)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>MVC veya Web API kurulmuş öğe ekleme

İskele eklemek için proje veya proje içindeki bir klasörü sağ tıklatın ve seçin **Ekle** – **yeni iskele kurulmuş öğe**aşağıdaki görüntüde gösterildiği gibi.

![İskele öğesi ekleme](aspnet-scaffolding-overview/_static/image1.png)

Gelen **İskele Ekle** penceresinde eklemek için iskele türünü seçin.

![İskele türünü seçin](aspnet-scaffolding-overview/_static/image2.png)

**Denetleyici Ekle** penceresi Entity Framework 6 yeni zaman uyumsuz özelliklerinden kullanmak mı istediğinizi de dahil olmak üzere, denetleyici oluşturma seçeneklerini belirlemek için fırsatı sağlar.

![Denetleyici ekleme](aspnet-scaffolding-overview/_static/image3.png)

Sayfalar ve ilgili sınıfların senaryonuz için oluşturulur. Örneğin, aşağıdaki resimde MVC denetleyicisi ve filmler adlı bir model sınıfı için yapı iskelesi ile oluşturulan görünümleri gösterir.

![Oluşturulan dosyalar](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Web formları için kurulmuş Öğe Ekle

Web Forms kod oluşturur yapı iskelesi eklemek için bir uzantı için Visual Studio yüklemek veya MVC bağımlılıkları ekleyin gerekir. Her iki yaklaşımın aşağıda gösterilmektedir, ancak yalnızca bu yaklaşımlardan birini yapmanız gerekir.

### <a name="web-forms-scaffolding-extension"></a>Web Forms uzantısı iskele kurma

Web Forms projeyle yapı iskelesi kullanmanıza olanak sağlayan bir Visual Studio uzantısı yükleyebilirsiniz. Visual Studio'da seçin **Araçları** ve ardından **Uzantılar ve güncelleştirmeler**. Visual Studio Galerisi için bu iletişim kutusundan arama **Web Forms İskele**.

![web forms iskele yükleyin](aspnet-scaffolding-overview/_static/image5.png)

Daha fazla bilgi için bkz: [Web Forms İskele](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>MVC bağımlılıkları

MVC bağımlılıkları eklemek için seçin **Ekle** - **yeni iskele kurulmuş öğe**. İskele Ekle penceresinde seçin **MVC bağımlılıkları**, aşağıda gösterildiği gibi.

![MVC bağımlılıkları ekleyin.](aspnet-scaffolding-overview/_static/image6.png)

MVC iskele için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketlerini ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneğini belirlerseniz MVC projesinde gerekli içerik dosyaların yanı sıra en az bağımlılıkları eklenir. Yapı iskelesi kolayca kullanmak için tam bağımlılıkları seçin.

![Tam bağımlılıkları seçin](aspnet-scaffolding-overview/_static/image7.png)

Göreceğiniz bağımlılıkları eklendikten sonra bir **readme.txt** dosya. Dikkatle projenizi düzgün çalıştığından emin olmak için bu dosyayı'ndaki yönergeleri izleyin.

Readme.txt dosyasında adımlarını tamamladıktan sonra MVC ve Web API hakkında önceki bölümde gösterildiği gibi yeni iskele kurulmuş öğe ekleyebilirsiniz. Otomatik olarak oluşturulan görünümleri ve denetleyici projenizi içinde düzgün çalışacaktır.

## <a name="tutorials"></a>Öğreticiler

Özelleştirilmiş iskele kurucu oluşturmak için bkz: [Visual Studio için özel iskele kurucu oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Oluşturulan dosyalar özelleştirmek için bkz: [yeni iskele kurulmuş öğe iletişim oluşturulan dosyalardan özelleştirmek nasıl](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Yapı iskelesi ile kullanma örneği için **Database First geliştirme**, bkz: [EF veritabanı ilk ASP.NET MVC ile](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Yapı iskelesi içinde kullanma örneği için bir **MVC** proje için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).

Yapı iskelesi içinde kullanma örneği için bir **Web API** proje için bkz: [özniteliği yönlendirme Web API 2'deki bir REST API'si oluşturma](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
