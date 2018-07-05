---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Web API'ye çağrı yapma bir Windows Phone 8 uygulama (C#) | Microsoft Docs
author: rmcmurray
description: Bir Windows Phone 8 uygulaması için bir kitap Kataloğu sağlayan bir ASP.NET Web API uygulaması oluşan eksiksiz bir uçtan uca senaryo oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 40be2935c52e7dcab9e682d4d15e9e75c0260223
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388428"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>(C#) bir Windows Phone 8 uygulamasından Web API'si çağırma
====================
tarafından [Robert McMurray '](https://github.com/rmcmurray)

Bu öğreticide, bir Windows Phone 8 uygulaması için bir kitap Kataloğu sağlayan bir ASP.NET Web API uygulaması oluşan eksiksiz bir uçtan uca senaryo nasıl oluşturulacağını öğreneceksiniz.

### <a name="overview"></a>Genel Bakış

RESTful Hizmetleri gibi ASP.NET Web API HTTP tabanlı uygulamaların geliştiricileri için oluşturma, sunucu tarafı ve istemci tarafı uygulamalarına yönelik mimari özetleyen tarafından basitleştirin. İletişim için özel bir yuva tabanlı protokol oluşturmak yerine, Web API'si, geliştiricilerin yalnızca, uygulama için gerekli HTTP yöntemleri yayımlamanız gerekir (örneğin: GET, POST, PUT, DELETE), ve istemci uygulaması geliştiricileri yeterlidir kullanma Kullanıcıların uygulama için gerekli HTTP yöntemleri.

Bu uçtan uca öğreticide Web API'sini aşağıdaki projeleri oluşturmak için nasıl kullanılacağını öğreneceksiniz:

- İçinde [Bu öğreticinin ilk bölümünü](#STEP1), bir kitap Kataloğu yönetmek için oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerinin tümünü destekleyen bir ASP.NET Web API uygulaması oluşturacaksınız. Bu uygulamanızın kullanacağı [örnek XML dosyası (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) MSDN'den.
- İçinde [Bu öğreticinin ikinci](#STEP2), Web API uygulamanızı verileri alır etkileşimli bir Windows Phone 8 uygulaması oluşturacaksınız.

#### <a name="prerequisites"></a>Önkoşullar

- Windows Phone 8 SDK ile Visual Studio 2013
- Bir 64-bit sistemde yüklü Hyper-V ile Windows 8 veya daha sonra
- Ek gereksinimler listesi için bkz. *sistem gereksinimleri* bölümünde [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) indirme sayfası.

> [!NOTE]
> Web API ve Windows Phone 8 projelerinin yerel sisteminizde arasındaki bağlantıyı test etmek için kullanacaksanız, yönergeleri gerekecektir *[yerel bir Web API uygulamalarını Windows Phone 8 öykünücüsü'nü bağlanma Bilgisayar](https://go.microsoft.com/fwlink/?LinkId=324014)* makalenin test ortamınızı ayarlayın.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>1. adım: Web API kitaplığı projesi oluşturma

Bu uçtan uca öğreticinin ilk adımı, tüm CRUD işlemleri destekleyen bir Web API projesi oluşturmaktır; Bu çözümde için Windows Phone uygulaması projesi ekleyeceksiniz Not [2. adım](#STEP2) Bu öğreticinin.

1. Açık **Visual Studio 2013'ün**.
2. Tıklayın **dosya**, ardından **yeni**, ardından **proje**.
3. Zaman **yeni proje** iletişim kutusu görüntülenir, genişletme **yüklü**, ardından **şablonları**, ardından **Visual C#** ve ardından **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Genişletmek için tıklayın                                                                |


4. Vurgulama **ASP.NET Web uygulaması**, girin **Kitaplığı** proje adı ve ardından **Tamam**.
5. Zaman **yeni ASP.NET projesi** iletişim kutusu görüntülenirse, seçin **Web API** şablonu ve ardından **Tamam**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Genişletmek için tıklayın                                                                |


6. Web API projesi açıldığında örnek denetleyicisi projeden kaldır:

    1. Genişletin **denetleyicileri** Çözüm Gezgini'nde klasörü.
    2. Sağ **ValuesController.cs** dosya ve ardından **Sil**.
    3. Tıklayın **Tamam** silme işlemini onaylamanız istendiğinde.
7. Bir XML veri dosyası için Web API projesi ekleyin; Bu dosya kitaplığı katalog içeriğini içerir:

   1. Sağ **uygulama\_veri** klasör Çözüm Gezgini'nde, ardından **Ekle**ve ardından **yeni öğe**.
   2. Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, vurgulayın **XML dosyası** şablonu.
   3. Dosya adı **Books.xml**ve ardından **Ekle**.
   4. Zaman **Books.xml** dosya açıldığında, XML'den örnek kod dosyasında yerine **books.xml** MSDN'de dosyası: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. XML dosyasını kaydedip kapatın.

8. Kitaplığı modeli Web API projesine ekleme; Bu model, Kitaplığı'nı uygulama oluşturma, okuma, güncelleştirme ve silme (CRUD) mantığı içerir:

   1. Sağ **modelleri** klasör Çözüm Gezgini'nde, ardından **Ekle**ve ardından **sınıfı**.
   2. Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, sınıf dosyası adı **BookDetails.cs**ve ardından **Ekle**.
   3. Zaman **BookDetails.cs** dosya açıldığında, dosyanın kodu aşağıdakiyle değiştirin: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Kaydet ve Kapat **BookDetails.cs** dosya.

9. Web API projesi için kitaplığı denetleyici ekleyin:

   1. Sağ **denetleyicileri** klasör Çözüm Gezgini'nde, ardından **Ekle**ve ardından **denetleyicisi**.
   2. Zaman **İskele Ekle** iletişim kutusu görüntülenir, vurgulayın **Web API 2 denetleyici - boş**ve ardından **Ekle**.
   3. Zaman **denetleyici Ekle** iletişim kutusu görüntülenir, denetleyici adı **BooksController**ve ardından **Ekle**.
   4. Zaman **BooksController.cs** dosya açıldığında, dosyanın kodu aşağıdakiyle değiştirin: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Kaydet ve Kapat **BooksController.cs** dosya.

10. Hataları denetlemek için Web API uygulaması oluşturun.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>2. adım: Windows Phone 8 kitaplığı Kataloğu proje ekleme

Bu uçtan uca senaryonun sonraki adım, Windows Phone 8 için kataloğu uygulaması oluşturmaktır. Bu uygulamanızın kullanacağı *Windows Phone Databound uygulama* şablonu varsayılan kullanıcı arabirimi ve oluşturduğunuz Web API uygulamasını kullanacağı [1. adım](#STEP1) Bu öğreticinin veri kaynağı olarak.

1. Sağ **Kitaplığı** çözümde Çözüm Gezgini'nde, ardından **Ekle**, ardından **yeni proje**.
2. Zaman **yeni proje** iletişim kutusu görüntülenir, genişletme **yüklü**, ardından **Visual C#**, ardından **Windows Phone**.
3. Vurgulama **Windows Phone Databound uygulama**, girin **BookCatalog** adı ve ardından **Tamam**.
4. Json.NET NuGet paketini ekleme **BookCatalog** proje:

    1. Sağ **başvuruları** için **BookCatalog** Çözüm Gezgini'nde proje ve ardından **NuGet paketlerini Yönet**.
    2. Zaman **NuGet paketlerini Yönet** iletişim kutusu görüntülenir, genişletme **çevrimiçi** bölümünde ve vurgulama **nuget.org**.
    3. Girin **Json.NET** arama alanını ve arama simgesine tıklayın.
    4. Vurgulama **Json.NET** 'a tıklayın ve arama sonuçlarını **yükleme**.
    5. Yükleme tamamlandığında, tıklayın **Kapat**.
5. Ekleme **BookDetails** için model **BookCatalog** proje; bu kitaplığı sınıfının genel bir model içerir:

   1. Sağ **BookCatalog** proje Çözüm Gezgini'nde, ardından tıklatın **Ekle**ve ardından **yeni klasör**.
   2. Yeni klasör adı **modelleri**.
   3. Sağ **modelleri** klasör Çözüm Gezgini'nde, ardından **Ekle**ve ardından **sınıfı**.
   4. Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, sınıf dosyası adı **BookDetails.cs**ve ardından **Ekle**.
   5. Zaman **BookDetails.cs** dosya açıldığında, dosyanın kodu aşağıdakiyle değiştirin: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Kaydet ve Kapat **BookDetails.cs** dosya.

6. Güncelleştirme **MainViewModel.cs** sınıf kitaplığı Web API'sini uygulama ile iletişim kurmak için işlevselliği eklemek için:

   1. Genişletin **Viewmodel'lar** Çözüm Gezgini gittikten sonra çift tıklayarak klasöründe **MainViewModel.cs** dosya.
   2. Zaman **MainViewModel.cs** dosya açıldığında, dosyanın kodu aşağıdakiyle değiştirin; değerini güncelleştirmek gerektiğine dikkat edin `apiUrl` sabit gerçek URL Web API'niz ile: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Kaydet ve Kapat **MainViewModel.cs** dosya.

7. Güncelleştirme **MainPage.xaml** uygulama adı özelleştirmek için dosya:

   1. Çift **MainPage.xaml** Çözüm Gezgini'nde dosya.
   2. Zaman **MainPage.xaml** dosya açıldığında, aşağıdaki kod satırlarını bulun: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Bu satırları aşağıdaki satırla değiştirin: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Kaydet ve Kapat **MainPage.xaml** dosya.

8. Güncelleştirme **DetailsPage.xaml** görüntülenen öğelerini özelleştirmek için dosya:

   1. Çift **DetailsPage.xaml** Çözüm Gezgini'nde dosya.
   2. Zaman **DetailsPage.xaml** dosya açıldığında, aşağıdaki kod satırlarını bulun: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Bu satırları aşağıdaki satırla değiştirin: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Kaydet ve Kapat **DetailsPage.xaml** dosya.

9. Hataları denetlemek için Windows Phone uygulaması oluşturun.

### <a name="step-3-testing-the-end-to-end-solution"></a>3. adım: uçtan uca çözüm test ediliyor

Belirtildiği gibi *önkoşulları* bölümü Web API ve Windows Phone 8 arasındaki bağlantıyı Test ettiğinizde bu öğreticinin projeleri yerel sisteminizde, yönergeleri gerekecektir *[ Yerel bir bilgisayardaki Web API uygulamaları Windows Phone 8 öykünücüsü'nü bağlanma](https://go.microsoft.com/fwlink/?LinkId=324014)* makalenin test ortamınızı ayarlayın.

Yapılandırılmış bir test ortamı oluşturduktan sonra Windows Phone uygulaması başlangıç projesi olarak ayarlamak gerekir. Bunu yapmak için vurgulayın **BookCatalog** Çözüm Gezgini ve ardından uygulama **başlangıç projesi olarak ayarla**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Genişletmek için tıklayın |

F5 tuşuna bastığınızda, Visual Studio hem de Windows Phone görüntüler öykünücüsünü başlar bir &quot;bekleyin&quot; Web API'nizi uygulama verilerinin alındığı sırada ileti:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Genişletmek için tıklayın |

Her şeyi başarılı olursa, görüntülenen Kataloğu görmeniz gerekir:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Genişletmek için tıklayın |

Herhangi bir kitap başlığında dokunursanız, uygulama kitap açıklamasını görüntüler:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Genişletmek için tıklayın |

Uygulamayı Web API'niz ile iletişim kuramıyorsa, bir hata iletisi görüntülenir:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Genişletmek için tıklayın |

Hata iletisinde dokunursanız, hata hakkında ek ayrıntılar görüntülenir:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Genişletmek için tıklayın                                                                 |

