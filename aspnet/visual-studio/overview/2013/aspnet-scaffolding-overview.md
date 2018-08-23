---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Visual Studio 2013'te ASP.NET iskeleti oluşturma | Microsoft Docs
author: tfitzmac
description: ASP.NET iskeleti oluşturma Visual Studio 2013'te bulunan yeni bir özelliktir.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755180"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Visual Studio 2013'te ASP.NET iskeleti oluşturma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET iskeleti oluşturma Visual Studio 2013'te bulunan yeni bir özelliktir.


## <a name="overview"></a>Genel Bakış

ASP.NET iskeleti oluşturma bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Visual Studio 2013, MVC ve Web API projeleri için önceden yüklenmiş kod oluşturucuları içerir. Hızlı bir şekilde veri modelleri ile etkileşime giren kodu eklemek istediğinizde yapı iskelesi projenize ekleyin. Yapı iskelesini kullanmaya projenizdeki standart veri işlemlerini geliştirmek için gereken süreyi azaltabilir.

Varsayılan olarak, Visual Studio 2013 Web formları projesi için kod desteklemez, ancak yapı iskelesi MVC bağımlılıkları projeye eklenirken veya bir uzantının yüklenmesi Web Forms ile kullanabilirsiniz. Her iki yaklaşım aşağıda gösterilmektedir.

Visual Studio 2013 güncelleştirme 2 (şu anda RC), ASP.NET, senaryonuzun gereksinimlerini karşılamak için yapı İskelesi genişletme olanağı sağlar. Bu işlevsellik ile özelleştirilmiş yapı iskelesi şablonu oluşturmak ve yeni İskele Ekle iletişim kutusuna ekleyin. Özelleştirilmiş şablon içerisinde, iskele kurulmuş öğe ekleme sırasında oluşturulan kodu belirtin. Daha fazla bilgi için [için Visual Studio bir özel destek oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Önkoşullar

ASP.NET iskeleti oluşturma kullanmak için şunlara sahip olmalısınız:

- Microsoft Visual Studio 2013
- Web geliştirici araçları (varsayılan Visual Studio 2013 yüklemesinin bir parçası)
- ASP.NET Web çerçeveleri ve araçları 2013'ü (varsayılan Visual Studio 2013 yüklemesinin bir parçası)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>MVC veya Web API'si için iskele kurulmuş Öğe Ekle

İskele eklemek, proje veya proje içinde bir klasöre sağ tıklayın ve seçin **Ekle** – **yeni iskele kurulmuş öğe**, aşağıdaki görüntüde gösterildiği gibi.

![İskele öğesi Ekle](aspnet-scaffolding-overview/_static/image1.png)

Gelen **İskele Ekle** penceresinde eklemek için iskele türünü seçin.

![İskele türünü seçin](aspnet-scaffolding-overview/_static/image2.png)

**Denetleyici Ekle** penceresi denetleyici, Entity Framework 6'dan yeni zaman uyumsuz özelliklerinin isteyip istemediğinizi dahil oluşturma seçeneklerini belirlemek için bir fırsat sağlar.

![Denetleyici ekleme](aspnet-scaffolding-overview/_static/image3.png)

İlgili sınıflar ve sayfaları senaryonuz için oluşturulur. Örneğin, aşağıdaki görüntüde ve MVC denetleyici filmler adlı bir model sınıfı için yapı iskelesi ile oluşturulan görünümleri gösterir.

![Oluşturulan dosyalar](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Web formları için iskele kurulmuş Öğe Ekle

Web Forms kodunu üretir yapı iskelesi eklemek için Visual Studio için bir uzantı yüklemeniz veya MVC bağımlılıkları ekleyin. Her iki yaklaşım aşağıda gösterilmektedir, ancak sadece Bu yaklaşımlardan birini yapmanız gerekir.

### <a name="web-forms-scaffolding-extension"></a>Web Forms uzantısı iskele kurma özelliği

Yapı iskelesi ile bir Web formları projesi kullanmanıza olanak sağlayan Visual Studio uzantısı yükleyebilirsiniz. Visual Studio'da **Araçları** ardından **Uzantılar ve güncelleştirmeler**. Visual Studio Galerisi için bu iletişim kutusundan arama **Web Forms yapı İskelesi**.

![web forms iskele kurma özelliği yükleme](aspnet-scaffolding-overview/_static/image5.png)

Daha fazla bilgi için [Web Forms yapı İskelesi](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>MVC bağımlılıkları

MVC bağımlılıkları eklemek için seçin **Ekle** - **yeni iskele kurulmuş öğe**. İskele Ekle penceresinde **MVC bağımlılıkları**, aşağıda gösterildiği gibi.

![MVC bağımlılıkları Ekle](aspnet-scaffolding-overview/_static/image6.png)

MVC yapı iskelesini oluşturmak için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketleri ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneği seçerseniz bir MVC projesi için gerekli içerik dosyalarının yanı sıra en az bağımlılıklar eklenir. Yapı iskelesi kolayca kullanmak için tam bağımlılıkları seçin.

![Tam bağımlılıkları seçin](aspnet-scaffolding-overview/_static/image7.png)

Göreceğiniz bağımlılıkları ekledikten sonra bir **readme.txt** dosya. Dikkatli bir şekilde, projenizin doğru şekilde çalıştığından emin olmak için bu dosyasındaki yönergeleri izleyin.

Readme.txt dosyasını adımları tamamladıktan sonra MVC ve Web API'si hakkında önceki bölümde gösterildiği gibi yeni iskele kurulmuş öğe ekleyebilirsiniz. Denetleyici ve otomatik olarak oluşturulan görünümler içinde proje düzgün çalışmayacaktır.

## <a name="tutorials"></a>Öğreticiler

Özelleştirilmiş bir iskele kurucu oluşturmak için bkz [için Visual Studio bir özel destek oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Oluşturulan dosyalar özelleştirmek için bkz: [yeni iskele kurulmuş öğe iletişim oluşturulan dosyalardan özelleştirme](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Yapı iskelesi ile kullanma örneği için **veritabanı ilk geliştirme**, bkz: [ASP.NET MVC ile EF veritabanı ilk](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Yapı iskelesi içinde kullanma örneği için bir **MVC** projesine [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).

Yapı iskelesi içinde kullanma örneği için bir **Web API** projesine [Web API 2'de öznitelik yönlendirme ile REST API oluşturma](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
