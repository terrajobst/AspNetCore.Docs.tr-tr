---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework mocking zaman birim testi ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: "Bu yönerge ve uygulama Entity Framework kullanan Web API 2 uygulamanız için birim testleri oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Nasıl değiştirileceğini gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 2d8a3df94c91d2fac79006916375764c2b90dc85
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Entity Framework mocking zaman birim ASP.NET Web API 2 testi
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Bu yönerge ve uygulama Entity Framework kullanan Web API 2 uygulamanız için birim testleri oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Test etmek için bir bağlam nesnesi geçirme etkinleştirmek için kurulmuş denetleyicisi değiştirme ve Entity Framework ile çalışma test nesneleri oluşturma gösterir.
> 
> ASP.NET Web API ile test birim giriş için bkz: [birim testi ASP.NET Web API 2 ile](unit-testing-with-aspnet-web-api.md).
> 
> Bu öğreticide, ASP.NET Web API temel kavramları bildiğinizi varsayar. Giriş bir öğretici için bkz: [ASP.NET Web API 2 ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>Bu konuda

Bu konu aşağıdaki bölümleri içermektedir:

- [Önkoşulları](#prereqs)
- [Kodu indirme](#download)
- [Birim testi projesi ile uygulama oluşturma](#appwithunittest)
- [Model sınıfı oluşturma](#modelclass)
- [Denetleyici ekleme](#controller)
- [Bağımlılık ekleme Ekle](#dependency)
- [Test projesinde NuGet paketi yüklemesi](#testpackages)
- [Test bağlamı oluşturma](#testcontext)
- [Testleri oluşturma](#tests)
- [Testleri çalıştırma](#runtests)

' Ndaki adımları zaten tamamladıysanız [birim testi ASP.NET Web API 2 ile](unit-testing-with-aspnet-web-api.md), bölümüne atlayabilirsiniz [denetleyiciyi ekleme](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2017 Community, Professional veya Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Kodu indirme

Karşıdan [projeyi](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Birim testi kodu ve bu konu için indirilebilir proje içeriyor [birim testi ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) konu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Birim testi projesi ile uygulama oluşturma

Uygulamanızı oluştururken, bir birim testi projesi oluşturma veya varolan bir uygulamaya birim testi projesi ekleyin. Bu öğreticide, uygulama oluştururken birim testi projesi oluşturma gösterilir.

Adlı yeni bir ASP.NET Web uygulaması oluşturma **StoreApp**.

Yeni ASP.NET projesi Windows'da seçin **boş** şablon klasörleri ekleyin ve Web API başvuru çekirdek. Seçin **birim testleri ekleme** seçeneği. Birim testi projesi otomatik olarak adlı **StoreApp.Tests**. Bu adı kullanmaya devam edebilir.

![Birim testi projesi oluşturma](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Uygulamayı oluşturduktan sonra iki proje - içerdiği görürsünüz **StoreApp** ve **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Model sınıfı oluşturma

Bir sınıf dosyaya StoreApp projenize eklemek **modelleri** adlı klasörü **Product.cs**. Dosya içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Çözümü oluşturun.

<a id="controller"></a>
## <a name="add-the-controller"></a>Denetleyici ekleme

Denetleyicileri klasörüne sağ tıklayıp **Ekle** ve **yeni iskele kurulmuş öğe**. Entity Framework kullanarak Eylemler ile Web API 2 denetleyicisi seçin.

![Yeni denetleyici ekleyin](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Aşağıdaki değerleri ayarlayın:

- Denetleyici adı: **ProductController**
- Model sınıfı: **ürün**
- Veri bağlamı sınıfı: [seçin **yeni veri bağlamı** aşağıda görüldüğü değerleri doldurur düğmesinin]

![Denetleyici belirtin](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Tıklatın **Ekle** denetleyicisi otomatik olarak oluşturulan kodu ile oluşturmak için. Kod oluşturma, alma, güncelleştirme ve ürün sınıfının örneklerini silmek için yöntemler içerir. Aşağıdaki kod yöntemini gösterir için bir ürün ekleyin. Yöntem örneği döndürür fark **Ihttpactionresult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

Ihttpactionresult Web API 2'deki yeni özelliklerden biridir ve birim testi geliştirme basitleştirir.

Sonraki bölümde kolaylaştırmak için oluşturulan kodu özelleştireceğiniz denetleyiciye test nesneleri geçirme.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Bağımlılık ekleme Ekle

Şu anda ProductController StoreAppContext sınıfının bir örneğini kullanmak için kodlanmış bir sınıftır. Uygulamanızı değiştirmek ve bu sabit kodlanmış bağımlılığı kaldırmak için bağımlılık ekleme adlı bir desen kullanır. Bu bağımlılık ayırarak, sahte nesnesinde sınarken geçirebilirsiniz.

Sağ **modelleri** klasörünü ve adlı yeni bir arabirim eklemek **IStoreAppContext**.

Kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

StoreAppContext.cs dosyasını açın ve aşağıdaki vurgulanan değişiklikleri yapın. Dikkat edilecek önemli değişiklikler şunlardır:

- StoreAppContext sınıfı IStoreAppContext arabirimini uygulayan
- Uygulanan MarkAsModified yöntemi


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

ProductController.cs dosyasını açın. Var olan kodu vurgulanmış kodu eşleşecek şekilde değiştirin. Bu değişiklikler StoreAppContext üzerinde bağımlılık bölme ve farklı bir nesne için bağlam sınıfını geçirmek diğer sınıflarını etkinleştirin. Bu değişiklik birim testleri sırasında bir test bağlamında geçirmenize olanak sağlar.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

ProductController yapmanız gereken tek daha fazla değişiklik yoktur. İçinde **PutProduct** yöntemi, varlık durumu ayarlar için satır değiştiren MarkAsModified yöntemine bir çağrı ile değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Çözümü oluşturun.

Artık test projesi kurmanız hazırsınız.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Test projesinde NuGet paketi yüklemesi

Bir uygulama oluşturmak için boş şablonunu kullandığınızda, birim testi projesi (StoreApp.Tests) yüklü herhangi bir NuGet paketinin içermez. Web API şablonu gibi diğer şablonları birim testi projesi bazı NuGet paketleri içerir. Bu öğretici için Entity Framework packge ve test projesi için Microsoft ASP.NET Web API 2 Çekirdek paket eklemeniz gerekir.

StoreApp.Tests projesine sağ tıklatın ve **NuGet paketlerini Yönet**. Paketler bu projeye eklemek için StoreApp.Tests proje seçmeniz gerekir.

![paketlerini yönetme](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Çevrimiçi paketlerinden bulun ve EntityFramework paketi (sürüm 6.0 veya üstü) yükleyin. EntityFramework paket zaten yüklü olduğunu görünürse, yerine StoreApp proje seçmiş olabilirsiniz StoreApp.Tests projesi.

![Entity Framework ekleme](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Bul ve Microsoft ASP.NET Web API 2 Çekirdek paketi yükleyin.

![Web API core paketini yükle](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

NuGet paketlerini Yönet penceresini kapatın.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Test bağlamı oluşturma

Adlı bir sınıf ekleyin **TestDbSet** test projesi için. Bu sınıf, sınama veri kümesi için temel sınıf olarak görev yapar. Kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Adlı bir sınıf ekleyin **TestProductDbSet** aşağıdaki kodu içeren test projesi için.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Adlı bir sınıf ekleyin **TestStoreAppContext** ve var olan kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Testleri oluşturma

Varsayılan olarak, test projenizin adlı boş test dosyasının içerir **UnitTest1.cs**. Bu dosya, test yöntemleri oluşturmak için kullandığınız öznitelikleri gösterir. Bu öğretici için yeni bir test sınıf ekleyeceksiniz çünkü bu dosyayı silebilirsiniz.

Adlı bir sınıf ekleyin **TestProductController** test projesi için. Kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Testleri çalıştırma

Şimdi testleri çalıştırmak hazırsınız. Tüm işaretlenir yöntemi **TestMethod** özniteliği test edilmiş. Gelen **Test** menü öğesi, testleri çalıştırın.

![testleri çalıştırma](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Açık **Test Gezgini** penceresinde ve test sonuçlarını dikkat edin.

![test sonuçları](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
