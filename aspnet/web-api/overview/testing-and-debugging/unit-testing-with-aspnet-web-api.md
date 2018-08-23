---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Birim testi ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: Bu kılavuzu ve uygulama, Web API 2 uygulama için basit birim testleri oluşturma işlemini göstermektedir. Bu öğreticide, bir birim test proj içerecek şekilde gösterilmektedir...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0a3f9ddde725995359de85c5e0783106f5de02bd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753909"
---
<a name="unit-testing-aspnet-web-api-2"></a>Birim testi ASP.NET Web API 2
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

[Projeyi yükle](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Bu kılavuzu ve uygulama, Web API 2 uygulama için basit birim testleri oluşturma işlemini göstermektedir. Bu öğreticide, bir denetleyici yönteminden döndürülen değerleri kontrol test yöntemler yazmak ve birim testi projesi içeren çözümünüze gösterilmektedir.
> 
> Bu öğreticide, ASP.NET Web API ile ilgili temel kavramlar hakkında bilgi sahibi olduğunuz varsayılır. Giriş niteliğindeki bir eğitim için bkz. [ASP.NET Web API 2 ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Birim testleri bu konuda, basit veri senaryoları için kasıtlı olarak sınırlıdır. Birim testi daha gelişmiş veri senaryoları için bkz. [sahte Entity Framework, birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>Bu konuda

Bu konu aşağıdaki bölümleri içermektedir:

- [Önkoşulları](#prereqs)
- [Kodu indir](#download)
- [Uygulama ile birim testi projesi oluşturma](#appwithunittest)

    - [Uygulama oluştururken, birim testi projesi ekleyin.](#whencreate)
    - [Birim testi projesi varolan bir uygulamaya ekleme](#addtoexisting)
- [Web API 2 uygulama ayarlama](#setupproject)
- [Test projesinde NuGet paketlerini yükleme](#testpackages)
- [Testleri oluşturma](#tests)
- [Testleri çalıştırın](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2017 Community, Professional veya Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Kodu indir

İndirme [projeyi](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Birim testi kodu ve için bu konunun indirilebilir proje içerir [sahte Entity Framework, ASP.NET Web API birim testi](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) konu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Uygulama ile birim testi projesi oluşturma

Uygulamanızı oluştururken bir birim test projesi oluşturun veya mevcut bir uygulamaya bir birim test projesi ekleyin. Bu öğreticide, bir birim test projesi oluşturmak için her iki yöntem de gösterilir. Bu öğreticiyi uygulamak için her iki yöntemle kullanabilirsiniz.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Uygulama oluştururken, birim testi projesi ekleyin.

Adlı yeni bir ASP.NET Web uygulaması oluşturma **StoreApp**.

![Proje oluşturma](unit-testing-with-aspnet-web-api/_static/image1.png)

Yeni ASP.NET projesi Windows'da seçin **boş** şablon klasörleri ekleyin ve Web API'si için başvuru çekirdek. Seçin **birim testleri ekleme** seçeneği. Birim test projesi otomatik olarak adlandırılır **StoreApp.Tests**. Bu ad tutabilirsiniz.

![Birim testi projesi oluşturma](unit-testing-with-aspnet-web-api/_static/image2.png)

Uygulamayı oluşturduktan sonra iki proje içeren görürsünüz.

![iki proje](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Birim testi projesi varolan bir uygulamaya ekleme

Uygulamanızı oluştururken birim test projesi oluşturmadıysanız herhangi bir zamanda ekleyebilirsiniz. Örneğin, zaten sahip olduğunuz StoreApp adlı bir uygulama ve birim testleri eklemek istediğiniz varsayalım. Birim testi projesi eklemek için çözümü sağ tıklatın ve seçin **Ekle** ve **yeni proje**.

![çözüme yeni proje Ekle](unit-testing-with-aspnet-web-api/_static/image4.png)

Seçin **Test** sol bölmesinde, seçip **birim testi projesi** proje türü. Projeyi adlandırın **StoreApp.Tests**.

![Birim testi projesi ekleme](unit-testing-with-aspnet-web-api/_static/image5.png)

Birim test projesi çözümünüze görürsünüz.

Birim test projesinde özgün proje için bir proje başvurusu ekleyin.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Web API 2 uygulama ayarlama

StoreApp projeniz için sınıf dosyası ekleyin **modelleri** adlı klasöre **Product.cs**. Dosyanın içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Çözümü oluşturun.

Denetleyicileri klasörüne sağ tıklayıp **Ekle** ve **yeni iskele kurulmuş öğe**. Seçin **Web API 2 denetleyici - boş**.

![Yeni denetleyici ekleyin](unit-testing-with-aspnet-web-api/_static/image6.png)

Denetleyici adı kümesine **SimpleProductController**, tıklatıp **Ekle**.

![Denetleyici belirtin](unit-testing-with-aspnet-web-api/_static/image7.png)

Varolan kodu aşağıdaki kodla değiştirin. Bu örneği basitleştirmek amacıyla verileri bir veritabanı yerine bir liste içinde depolanır. Bu sınıfta tanımlanan liste üretim verileri temsil eder. Denetleyici ürün nesnelerin bir listesini bir parametre olarak alan bir oluşturucu içerdiğine dikkat edin. Bu oluşturucu, test verileri geçirmenizi sağlar, birim testi. İki denetleyici de içeren **zaman uyumsuz** birim testi zaman uyumsuz yöntemleri göstermek için yöntemleri. Bu zaman uyumsuz yöntemler çağrılarak uygulandığına **Task.FromResult** gereksiz kod, ancak genellikle yöntemleri en aza indirmek için kaynak kullanımı yoğun işlemleri içerir.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Örneğini GetProduct yöntemi döndürür **Ihttpactionresult** arabirimi. Web API 2'deki yeni özelliklerin Ihttpactionresult biridir ve birim testi geliştirmenin kolaylaştırır. Ihttpactionresult arabirimi uygulayan sınıflar bulunur [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) ad alanı. Bu sınıfların bir eylem istek olası yanıtlar temsil eder ve bunlar için HTTP durum kodları karşılık gelir.

Çözümü oluşturun.

Şimdi test projesini ayarlarsınız hazırsınız.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Test projesinde NuGet paketlerini yükleme

Bir uygulama oluşturmak için boş şablonu kullandığınızda, birim testi projesi (StoreApp.Tests) yüklü herhangi bir NuGet paketinin içermez. Web API şablonu gibi diğer şablonları, birim test projesinde NuGet paketlerinden bazıları içerir. Bu öğreticide, Microsoft ASP.NET Web API 2 Çekirdek paketini test projesine eklemeniz gerekir.

StoreApp.Tests projeye sağ tıklayıp **NuGet paketlerini Yönet**. Paketler bu projeye eklemek için StoreApp.Tests proje seçmeniz gerekir.

![paketlerini yönetme](unit-testing-with-aspnet-web-api/_static/image8.png)

Bulun ve Microsoft ASP.NET Web API 2 Çekirdek paketini yükleyin.

![Web API çekirdek paketini yükle](unit-testing-with-aspnet-web-api/_static/image9.png)

NuGet paketlerini Yönet penceresini kapatın.

<a id="tests"></a>
## <a name="create-tests"></a>Testleri oluşturma

Varsayılan olarak, test projenize UnitTest1.cs adlı boş bir test dosyası içerir. Bu dosya, test yöntemleri oluşturduğunuzda kullandığınız öznitelikleri gösterir. Birim testleriniz için için bu dosyayı kullanabilir veya kendi dosyanızı oluşturun.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Bu öğreticide, kendi test sınıfı oluşturur. UnitTest1.cs dosyasını silebilirsiniz. Adlı bir sınıf ekleyin **TestSimpleProductController.cs**, kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Testleri çalıştırma

Testleri çalıştırmak artık hazırsınız. Tüm ile işaretlenmiş yöntem **TestMethod** özniteliği test edilmiş. Gelen **Test** menü öğesi, testleri çalıştırın.

![testleri çalıştırma](unit-testing-with-aspnet-web-api/_static/image11.png)

Açık **Test Gezgini** penceresinde ve test sonuçlarını dikkat edin.

![test sonuçları](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Özet

Bu öğreticiyi tamamladınız. Bu öğreticide verileri kasıtlı olarak birim testi koşullar odaklanmak için Basitleştirilmiş. Birim testi daha gelişmiş veri senaryoları için bkz. [sahte Entity Framework, birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
