---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Sahte Entity Framework, birim testi ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: Bu kılavuzu ve uygulama Entity Framework kullanan, Web API 2 uygulaması için birim testleri oluşturma işlemini göstermektedir. Nasıl değiştirileceğini gösterir...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8945f913abe8fb8397d07a5994000fff2348f1f7
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795385"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Sahte Entity Framework, birim testi ASP.NET Web API 2
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

[Projeyi yükle](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Bu kılavuzu ve uygulama Entity Framework kullanan, Web API 2 uygulaması için birim testleri oluşturma işlemini göstermektedir. Bu, Entity Framework ile çalışma testi nesneleri oluşturma ve test etmek için bir bağlam nesnesi geçirerek etkinleştirmek için iskele kurulmuş denetleyicisini değiştirmek nasıl gösterir.
>
> Birim testi ile ASP.NET Web API'si için bir giriş için bkz [ASP.NET Web API 2 birim testiyle](unit-testing-with-aspnet-web-api.md).
>
> Bu öğreticide, ASP.NET Web API ile ilgili temel kavramlar hakkında bilgi sahibi olduğunuz varsayılır. Giriş niteliğindeki bir eğitim için bkz. [ASP.NET Web API 2 ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2

## <a name="in-this-topic"></a>Bu konuda

Bu konu aşağıdaki bölümleri içermektedir:

- [Önkoşulları](#prereqs)
- [Kodu indir](#download)
- [Uygulama ile birim testi projesi oluşturma](#appwithunittest)
- [Model sınıfı oluşturma](#modelclass)
- [Denetleyiciyi ekleme](#controller)
- [Bağımlılık ekleme Ekle](#dependency)
- [Test projesinde NuGet paketlerini yükleme](#testpackages)
- [Bu testin oluşturma](#testcontext)
- [Testleri oluşturma](#tests)
- [Testleri çalıştırın](#runtests)

' Ndaki adımları zaten tamamladıysanız [ASP.NET Web API 2 birim testiyle](unit-testing-with-aspnet-web-api.md), bölümüne atlayabilirsiniz [denetleyiciyi ekleme](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2017 Community, Professional veya Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Kodu indir

İndirme [projeyi](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Birim testi kodu ve için bu konunun indirilebilir proje içerir [birim testi ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) konu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Uygulama ile birim testi projesi oluşturma

Uygulamanızı oluştururken bir birim test projesi oluşturun veya mevcut bir uygulamaya bir birim test projesi ekleyin. Bu öğreticide, uygulama oluştururken bir birim testi projesi oluşturma gösterilmektedir.

Adlı yeni bir ASP.NET Web uygulaması oluşturma **StoreApp**.

Yeni ASP.NET projesi Windows'da seçin **boş** şablon klasörleri ekleyin ve Web API'si için başvuru çekirdek. Seçin **birim testleri ekleme** seçeneği. Birim test projesi otomatik olarak adlandırılır **StoreApp.Tests**. Bu ad tutabilirsiniz.

![Birim testi projesi oluşturma](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Uygulamayı oluşturduktan sonra iki proje - içerdiği görürsünüz **StoreApp** ve **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Model sınıfı oluşturma

StoreApp projeniz için sınıf dosyası ekleyin **modelleri** adlı klasöre **Product.cs**. Dosyanın içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Çözümü oluşturun.

<a id="controller"></a>
## <a name="add-the-controller"></a>Denetleyiciyi ekleme

Denetleyicileri klasörüne sağ tıklayıp **Ekle** ve **yeni iskele kurulmuş öğe**. Entity Framework kullanarak Eylemler ile Web API 2 denetleyicisi seçin.

![Yeni denetleyici ekleyin](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Aşağıdaki değerleri ayarlayın:

- Denetleyici adı: **ProductController**
- Model sınıfı: **ürün**
- Veri bağlamı sınıfı: [seçin **yeni veri bağlamı** aşağıda görüldüğü değerleri doldurur düğme]

![Denetleyici belirtin](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Tıklayın **Ekle** denetleyicisi otomatik olarak oluşturulan kod oluşturmak için. Kod oluşturma, alma, güncelleştirme ve ürün sınıfının örneklerini silme yöntemlerini içerir. Yöntemi aşağıdaki kodda gösterildiği bir ürün eklemek için. Yöntem örneği döndürdüğüne dikkat edin **Ihttpactionresult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

Web API 2'deki yeni özelliklerin Ihttpactionresult biridir ve birim testi geliştirmenin kolaylaştırır.

Sonraki bölümde kolaylaştırmak için oluşturulan kodu özelleştireceğim denetleyiciye test nesneleri geçirme.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Bağımlılık ekleme Ekle

Şu anda ProductController StoreAppContext sınıfının bir örneğini kullanmak için kodlanmış sınıftır. Uygulamanızı değiştirmeniz ve bu sabit kodlanmış bir bağımlılığı kaldırmak için bağımlılık ekleme adlı bir desen kullanır. Bu bağımlılık ayırarak, test ederken sahte bir nesne geçirebilirsiniz.

Sağ **modelleri** klasöründe ve adlı yeni bir arabirim ekleme **IStoreAppContext**.

Kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

StoreAppContext.cs dosyasını açın ve aşağıdaki vurgulanmış değişiklikleri yapın. Dikkat edilecek önemli değişiklikler şunlardır:

- StoreAppContext sınıfı IStoreAppContext arabirimini uygular.
- Uygulanan MarkAsModified yöntemi


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

ProductController.cs dosyasını açın. Varolan kodu vurgulanmış kodu eşleşecek şekilde değiştirin. Bu değişiklikler, bağımlılık StoreAppContext üzerinde kesme ve diğer sınıflar farklı bir nesne için bağlam sınıfını geçirmek etkinleştirin. Bu değişiklik sırasında birim testleri bir test bağlamında geçirmenize olanak tanır.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

ProductController yapmanız gereken daha fazla değişiklik yoktur. İçinde **PutProduct** yöntemi, varlık durumu ayarlar satır değiştiren MarkAsModified yöntemi çağrısı ile değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Çözümü oluşturun.

Şimdi test projesini ayarlarsınız hazırsınız.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Test projesinde NuGet paketlerini yükleme

Bir uygulama oluşturmak için boş şablonu kullandığınızda, birim testi projesi (StoreApp.Tests) yüklü herhangi bir NuGet paketinin içermez. Web API şablonu gibi diğer şablonları, birim test projesinde NuGet paketlerinden bazıları içerir. Bu öğreticide, Entity Framework packge ve Microsoft ASP.NET Web API 2 Çekirdek paketini test projesine eklemeniz gerekir.

StoreApp.Tests projeye sağ tıklayıp **NuGet paketlerini Yönet**. Paketler bu projeye eklemek için StoreApp.Tests proje seçmeniz gerekir.

![paketlerini yönetme](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Çevrimiçi paketlerinden bulun ve EntityFramework paketi (sürüm 6.0 veya üzeri) yükleyin. EntityFramework paket zaten yüklü olduğunu görüntüleniyorsa, StoreApp.Tests proje yerine StoreApp proje seçmiş olabilirsiniz.

![Entity Framework Ekle](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Bulun ve Microsoft ASP.NET Web API 2 Çekirdek paketini yükleyin.

![Web API çekirdek paketini yükle](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

NuGet paketlerini Yönet penceresini kapatın.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Bu testin oluşturma

Adlı bir sınıf ekleyin **TestDbSet** test projesi için. Bu sınıf, test veri kümeniz için temel sınıf olarak görev yapar. Kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Adlı bir sınıf ekleyin **TestProductDbSet** aşağıdaki kodu içeren bir test projesi için.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Adlı bir sınıf ekleyin **TestStoreAppContext** ve varolan kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Testleri oluşturma

Varsayılan olarak, adlı boş bir test dosyası test projenize içerir **UnitTest1.cs**. Bu dosya, test yöntemleri oluşturduğunuzda kullandığınız öznitelikleri gösterir. Bu öğretici için yeni bir test sınıfı ekleyeceksiniz çünkü bu dosyayı silebilirsiniz.

Adlı bir sınıf ekleyin **TestProductController** test projesi için. Kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Testleri çalıştırma

Testleri çalıştırmak artık hazırsınız. Tüm ile işaretlenmiş yöntem **TestMethod** özniteliği test edilmiş. Gelen **Test** menü öğesi, testleri çalıştırın.

![testleri çalıştırma](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Açık **Test Gezgini** penceresinde ve test sonuçlarını dikkat edin.

![test sonuçları](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
