---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Web API çağırma bir Windows Phone 8 uygulama (C#) | Microsoft Docs
author: rmcmurray
description: Bir Windows Phone 8 uygulaması defterlerine kataloğunu sağlayan bir ASP.NET Web API uygulaması oluşan eksiksiz bir uçtan uca senaryo oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874232"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Bir Windows Phone 8 uygulamasından (C#) Web API'si çağırma
====================
tarafından [Robert McMurray '](https://github.com/rmcmurray)

Bu öğreticide, bir Windows Phone 8 uygulaması defterlerine kataloğunu sağlayan bir ASP.NET Web API uygulaması oluşan eksiksiz bir uçtan uca senaryo oluşturmak öğreneceksiniz.

### <a name="overview"></a>Genel Bakış

ASP.NET Web API gibi rESTful hizmetlerini, sunucu tarafı ve istemci tarafı uygulamalar mimarisi özetleyen tarafından geliştiriciler için HTTP tabanlı uygulamaların oluşturulmasını basitleştirir. İletişim için bir özel yuva tabanlı Protokolü oluşturmak yerine, Web API geliştiricilerin kendi uygulama için gerekli HTTP yöntemleri yayımlamak yeterlidir (örneğin: GET, POST, PUT, DELETE), ve istemci uygulaması geliştiricileri yeterlidir kullanmak Kullanıcıların uygulama için gerekli HTTP yöntemleri.

Bu uçtan uca öğretici kapsamında, Web API aşağıdaki projeleri oluşturmak için nasıl kullanılacağını öğreneceksiniz:

- İçinde [Bu öğreticinin ilk bölümünü](#STEP1), tüm kitap kataloğunun yönetmek için oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemlerini destekleyen bir ASP.NET Web API uygulaması oluşturacaksınız. Bu uygulamayı kullanmak [örnek XML dosyası (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) MSDN'den.
- İçinde [ikinci bölümü bu öğreticinin](#STEP2), Web API uygulamanızı verileri alır etkileşimli bir Windows Phone 8 uygulaması oluşturacaksınız.

#### <a name="prerequisites"></a>Önkoşullar

- Windows Phone 8 SDK ile Visual Studio 2013
- 64-bit sistem yüklü Hyper-V ile Windows 8 veya sonraki bir sürümü
- Ek gereksinimler listesi için bkz *sistem gereksinimleri* bölümünde [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) sayfa indirin.

> [!NOTE]
> Web API ve Windows Phone 8 projeleri yerel sisteminizde arasındaki bağlantıyı sınamak için kullanacaksanız,'ndaki yönergeleri izleyin gerekir *[yerel bir Web API uygulamalarını Windows Phone 8 öykünücüsü bağlanma Bilgisayar](https://go.microsoft.com/fwlink/?LinkId=324014)* makale test ortamınızı ayarlayın.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>1. adım: Web API kitaplığı projesi oluşturma

Bu uçtan uca öğreticinin ilk adımı, tüm CRUD işlemleri destekleyen bir Web API projesi oluşturmaktır; Bu çözüm için Windows Phone Uygulama projesi ekleyeceğini unutmayın [2. adım](#STEP2) Bu öğreticinin.

1. Açık **Visual Studio 2013'ün**.
2. Tıklatın **dosya**, ardından **yeni**ve ardından **proje**.
3. Zaman **yeni proje** iletişim kutusu görüntülenir, genişletin **yüklü**, ardından **şablonları**, ardından **Visual C#** ve ardından **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Genişletmek için tıklayın                                                                |


4. Vurgula **ASP.NET Web uygulaması**, girin **kitap** proje adı ve ardından **Tamam**.
5. Zaman **yeni ASP.NET projesi** iletişim kutusu görüntülenir, seçin **Web API** şablonu ve ardından **Tamam**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Genişletmek için tıklayın                                                                |


6. Web API projesi açıldığında, örnek denetleyicisi projeden kaldırın:

    1. Genişletme **denetleyicileri** Çözüm Gezgininde klasör.
    2. Sağ **ValuesController.cs** dosya ve ardından **silmek**.
    3. Tıklatın **Tamam** silme işlemini onaylamanız istendiğinde.
7. XML veri dosyası, Web API projesine ekleyin; Bu dosya kitap katalog içeriğinin içerir:

   1. Sağ **uygulama\_veri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **yeni öğe**.
   2. Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, vurgulayın **XML dosyası** şablonu.
   3. Dosya adı **Books.xml**ve ardından **Ekle**.
   4. Zaman **Books.xml** dosya açıldığında, örnekten XML dosyasındaki kodu yerine **books.xml** MSDN'de dosyası: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. XML dosyasını kaydedip kapatın.

8. Kitap modelin Web API projesine ekleyin; Bu model Kitaplığı'nı uygulama oluşturma, okuma, güncelleştirme ve Sil (CRUD) mantığını içerir:

   1. Sağ **modelleri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **sınıfı**.
   2. Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, sınıf dosya adı **BookDetails.cs**ve ardından **Ekle**.
   3. Zaman **BookDetails.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Kaydet ve Kapat **BookDetails.cs** dosya.

9. Kitap denetleyicisi için Web API projesi ekleyin:

   1. Sağ **denetleyicileri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **denetleyicisi**.
   2. Zaman **İskele Ekle** iletişim kutusu görüntülenir, vurgulayın **Web API 2 denetleyici - boş**ve ardından **Ekle**.
   3. Zaman **denetleyici Ekle** iletişim kutusu görüntülenir, denetleyici adı **BooksController**ve ardından **Ekle**.
   4. Zaman **BooksController.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Kaydet ve Kapat **BooksController.cs** dosya.

10. Hataları denetlemek için Web API uygulaması oluşturma.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>2. adım: Windows Phone 8 kitap katalog projenize ekleme

Bu uçtan uca senaryoyu bir sonraki adım, Windows Phone 8 için katalog uygulama oluşturmaktır. Bu uygulamayı kullanmak *Windows Phone veriye bağlı uygulama* şablonu varsayılan kullanıcı arabirimi ve onu için oluşturduğunuz Web API uygulama kullanacağınız [1. adım](#STEP1) veri kaynağı olarak bu öğreticinin.

1. Sağ **kitap** çözümde Çözüm Gezgini'nde ardından **Ekle**ve ardından **yeni proje**.
2. Zaman **yeni proje** iletişim kutusu görüntülenir, genişletin **yüklü**, ardından **Visual C#** ve ardından **Windows Phone**.
3. Vurgula **Windows Phone veriye bağlı uygulama**, girin **BookCatalog** adı ve ardından **Tamam**.
4. Json.NET NuGet paketi ekleme **BookCatalog** proje:

    1. Sağ **başvuruları** için **BookCatalog** Çözüm Gezgini'nde proje ve ardından **NuGet paketlerini Yönet**.
    2. Zaman **NuGet paketlerini Yönet** iletişim kutusu görüntülenir, genişletin **çevrimiçi** bölümünde ve vurgulayın **nuget.org**.
    3. Girin **Json.NET** arama alan ve arama simgesine tıklayın.
    4. Vurgula **Json.NET** arama sonuçları ve ardından **yükleme**.
    5. Yükleme tamamlandığında, tıklayın **Kapat**.
5. Ekleme **BookDetails** için model **BookCatalog** proje; bu kitap sınıfının genel bir modeli içerir:

   1. Sağ **BookCatalog** Çözüm Gezgini'nde proje ve ardından **Ekle**ve ardından **yeni klasör**.
   2. Yeni bir klasör adı **modelleri**.
   3. Sağ **modelleri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **sınıfı**.
   4. Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, sınıf dosya adı **BookDetails.cs**ve ardından **Ekle**.
   5. Zaman **BookDetails.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Kaydet ve Kapat **BookDetails.cs** dosya.

6. Güncelleştirme **MainViewModel.cs** sınıf kitaplığı Web API uygulamayla iletişim kurmak için işlevselliğini içerir:

   1. Genişletme **ViewModels** Çözüm Gezgini ve çift tıklatarak klasöründe **MainViewModel.cs** dosya.
   2. Zaman **MainViewModel.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin; değerini güncelleştirmeniz gerekecektir Not `apiUrl` Web API gerçek URL'si ile sabit: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Kaydet ve Kapat **MainViewModel.cs** dosya.

7. Güncelleştirme **MainPage.xaml** uygulama adı özelleştirmek için dosya:

   1. Çift **MainPage.xaml** Çözüm Gezgini'nde dosya.
   2. Zaman **MainPage.xaml** dosya açıldığında, aşağıdaki kod satırlarını bulun: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Bu satırlar aşağıdakiyle değiştirin: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Kaydet ve Kapat **MainPage.xaml** dosya.

8. Güncelleştirme **DetailsPage.xaml** görüntülenen öğelerini özelleştirmek için dosya:

   1. Çift **DetailsPage.xaml** Çözüm Gezgini'nde dosya.
   2. Zaman **DetailsPage.xaml** dosya açıldığında, aşağıdaki kod satırlarını bulun: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Bu satırlar aşağıdakiyle değiştirin: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Kaydet ve Kapat **DetailsPage.xaml** dosya.

9. Hataları denetlemek için Windows Phone Uygulama oluşturun.

### <a name="step-3-testing-the-end-to-end-solution"></a>3. adım: uçtan uca çözüm test ediliyor

Bölümünde belirtildiği gibi *Önkoşullar* bölüm Web API ve Windows Phone 8 arasındaki bağlantıyı sınarken Bu öğreticinin yerel sisteminizde projeleri,'ndaki yönergeleri izleyin gerekecek *[ Yerel bir bilgisayarda Web API uygulamalarını Windows Phone 8 öykünücüsü bağlanma](https://go.microsoft.com/fwlink/?LinkId=324014)* makale test ortamınızı ayarlayın.

Yapılandırılmış test ortamını olduktan sonra Windows Phone Uygulama başlangıç projesi olarak ayarlamanız gerekir. Bunu yapmak için vurgulayın **BookCatalog** Çözüm Gezgini ve ardından uygulama **başlangıç projesi olarak ayarla**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Genişletmek için tıklayın |

F5 tuşuna bastığınızda, hem Windows Phone hangi görüntüler öykünücüsü, Visual Studio başlayacak bir &quot;bekleyin&quot; uygulama verileri, Web API öğesinden alındığı sırada ileti:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Genişletmek için tıklayın |

Her şeyi başarılı olursa, görüntülenen katalog görmeniz gerekir:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Genişletmek için tıklayın |

Tüm kitap başlığı dokunursanız, uygulama rehberi açıklama görüntülenir:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Genişletmek için tıklayın |

Uygulama, Web API'si ile iletişim kuramıyor, bir hata iletisi görüntülenir:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Genişletmek için tıklayın |

Hata iletisinde dokunursanız, hatayla ilgili ek ayrıntılar görüntülenir:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Genişletmek için tıklayın                                                                 |

