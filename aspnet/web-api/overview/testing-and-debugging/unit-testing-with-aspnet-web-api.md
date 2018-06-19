---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Birim testi ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: Bu yönerge ve uygulama, Web API 2 uygulamanız için basit birim testleri oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Bu öğretici, bir birim testi proj dahil gösterilmektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042751"
---
<a name="unit-testing-aspnet-web-api-2"></a>Birim testi ASP.NET Web API 2
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Bu yönerge ve uygulama, Web API 2 uygulamanız için basit birim testleri oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Bu öğretici, çözümünüzde birim testi projesi içerir ve bir denetleyici yönteminden döndürülen değerlerini denetleyin test yöntemleri yazma gösterilmektedir.
> 
> Bu öğreticide, ASP.NET Web API temel kavramları bildiğinizi varsayar. Giriş bir öğretici için bkz: [ASP.NET Web API 2 ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Birim testleri bu konuda, basit veri senaryoları için kasıtlı olarak sınırlıdır. Birim testi daha gelişmiş veri senaryoları için bkz: [Mocking Entity Framework zaman birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
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

    - [Uygulama oluştururken birim testi projesi ekleme](#whencreate)
    - [Varolan bir uygulamaya birim testi projesi ekleme](#addtoexisting)
- [Web API 2 uygulama ayarlama](#setupproject)
- [Test projesinde NuGet paketi yüklemesi](#testpackages)
- [Testleri oluşturma](#tests)
- [Testleri çalıştırma](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2017 Community, Professional veya Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Kodu indirme

Karşıdan [projeyi](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Birim testi kodu ve bu konu için indirilebilir proje içeriyor [Mocking Entity Framework zaman birim testi ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) konu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Birim testi projesi ile uygulama oluşturma

Uygulamanızı oluştururken, bir birim testi projesi oluşturma veya varolan bir uygulamaya birim testi projesi ekleyin. Bu öğreticide, bir birim testi projesi oluşturmak için her iki yöntemi gösterilir. Bu öğreticiyi izlemek için her iki yaklaşım kullanabilirsiniz.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Uygulama oluştururken birim testi projesi ekleme

Adlı yeni bir ASP.NET Web uygulaması oluşturma **StoreApp**.

![Proje oluşturma](unit-testing-with-aspnet-web-api/_static/image1.png)

Yeni ASP.NET projesi Windows'da seçin **boş** şablon klasörleri ekleyin ve Web API başvuru çekirdek. Seçin **birim testleri ekleme** seçeneği. Birim testi projesi otomatik olarak adlı **StoreApp.Tests**. Bu adı kullanmaya devam edebilir.

![Birim testi projesi oluşturma](unit-testing-with-aspnet-web-api/_static/image2.png)

Uygulamayı oluşturduktan sonra iki proje içeren görürsünüz.

![iki proje](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Varolan bir uygulamaya birim testi projesi ekleme

Uygulamanızı oluştururken birim testi projesi oluşturmadıysanız herhangi bir anda bir tane ekleyebilirsiniz. Örneğin, StoreApp adlı bir uygulama zaten var ve birim testleri eklemek istediğiniz varsayalım. Birim testi projesi eklemek için Çözümünüze sağ tıklayın ve seçin **Ekle** ve **yeni proje**.

![Yeni proje çözüme ekleyin](unit-testing-with-aspnet-web-api/_static/image4.png)

Seçin **Test** sol bölmede, seçip **birim testi projesi** proje türü için. Proje adı **StoreApp.Tests**.

![Birim testi projesi ekleme](unit-testing-with-aspnet-web-api/_static/image5.png)

Çözümünüzde birim testi projesi görürsünüz.

Birim testi projesi proje başvurusu özgün projeye ekleyin.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Web API 2 uygulama ayarlama

Bir sınıf dosyaya StoreApp projenize eklemek **modelleri** adlı klasörü **Product.cs**. Dosya içeriğini aşağıdaki kodla değiştirin.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Çözümü oluşturun.

Denetleyicileri klasörüne sağ tıklayıp **Ekle** ve **yeni iskele kurulmuş öğe**. Seçin **Web API 2 denetleyici - boş**.

![Yeni denetleyici ekleyin](unit-testing-with-aspnet-web-api/_static/image6.png)

Denetleyici adı ayarlamak **SimpleProductController**, tıklatıp **Ekle**.

![Denetleyici belirtin](unit-testing-with-aspnet-web-api/_static/image7.png)

Var olan kodu aşağıdaki kodla değiştirin. Bu örnek basitleştirmek için veriler, veritabanı yerine listesini depolanır. Bu sınıf içinde tanımlanan liste üretim verileri temsil eder. Denetleyicinin parametre olarak ürün nesnelerin bir listesini alan bir oluşturucu içerdiğine dikkat edin. Bu oluşturucu, test veri iletmek sağlar, birim testi. İki denetleyici ayrıca içerir **zaman uyumsuz** birim testi zaman uyumsuz yöntemleri göstermek için yöntemleri. Bu zaman uyumsuz yöntemleri çağırma uygulanan **Task.FromResult** yabancı kodu ancak normalde yöntemleri en aza indirmek için yoğun bir kaynak işlemlerinin içerir.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

GetProduct yöntemi örneğini döndürür **Ihttpactionresult** arabirimi. Ihttpactionresult Web API 2'deki yeni özelliklerden biridir ve birim testi geliştirme basitleştirir. İçinde bulunan Ihttpactionresult arabirimini uygulayan sınıflar [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) ad alanı. Bu sınıfların bir eylem isteği olası yanıtlarının temsil eder ve HTTP durum kodları karşılık.

Çözümü oluşturun.

Artık test projesi kurmanız hazırsınız.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Test projesinde NuGet paketi yüklemesi

Bir uygulama oluşturmak için boş şablonunu kullandığınızda, birim testi projesi (StoreApp.Tests) yüklü herhangi bir NuGet paketinin içermez. Web API şablonu gibi diğer şablonları birim testi projesi bazı NuGet paketleri içerir. Bu öğretici için oluşturduğunuz test projesinin Microsoft ASP.NET Web API 2 Çekirdek paketi eklemeniz gerekir.

StoreApp.Tests projesine sağ tıklatın ve **NuGet paketlerini Yönet**. Paketler bu projeye eklemek için StoreApp.Tests proje seçmeniz gerekir.

![paketlerini yönetme](unit-testing-with-aspnet-web-api/_static/image8.png)

Bul ve Microsoft ASP.NET Web API 2 Çekirdek paketi yükleyin.

![Web API core paketini yükle](unit-testing-with-aspnet-web-api/_static/image9.png)

NuGet paketlerini Yönet penceresini kapatın.

<a id="tests"></a>
## <a name="create-tests"></a>Testleri oluşturma

Varsayılan olarak, test projenizin UnitTest1.cs adlı bir boş test dosyası içerir. Bu dosya, test yöntemleri oluşturmak için kullandığınız öznitelikleri gösterir. Birim testleri için bu dosyayı kullanabilir veya kendi dosyanızı oluşturun.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Bu öğretici için kendi test sınıfı oluşturur. UnitTest1.cs dosyayı silebilirsiniz. Adlı bir sınıf ekleyin **TestSimpleProductController.cs**ve kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Testleri çalıştırma

Şimdi testleri çalıştırmak hazırsınız. Tüm işaretlenir yöntemi **TestMethod** özniteliği test edilmiş. Gelen **Test** menü öğesi, testleri çalıştırın.

![testleri çalıştırma](unit-testing-with-aspnet-web-api/_static/image11.png)

Açık **Test Gezgini** penceresinde ve test sonuçlarını dikkat edin.

![test sonuçları](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Özet

Bu öğretici tamamladınız. Bu öğreticide veri kasıtlı olarak birim testi koşullar odaklanmak Basitleştirilmiş. Birim testi daha gelişmiş veri senaryoları için bkz: [Mocking Entity Framework zaman birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
