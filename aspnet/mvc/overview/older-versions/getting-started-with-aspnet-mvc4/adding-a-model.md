---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e68b6d41c7880fb849bbefbedd4c4e25c6f47b0e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837765"
---
<a name="adding-a-model"></a>Model ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.


Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıflar ekleyeceksiniz. Bu sınıflar olacaktır &quot;modeli&quot; ASP.NET MVC uygulamasını bir parçası.

Bir .NET Framework Veri erişim teknolojisi olarak bilinen kullanacağınız [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) tanımlayın ve bu model sınıfları ile çalışmak için. Geliştirme paradigma adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*. Kod ilk basit sınıfları yazarak model nesneleri oluşturmanızı sağlar. (Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnesi.&quot;) Ardından, bir çok temiz ve hızlı geliştirme iş akışını sağlayan çalışma sırasında sınıflardan oluşturduğunuz veritabanına sahip olabilir.

## <a name="adding-model-classes"></a>Model sınıfları ekleme

İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.

![](adding-a-model/_static/image1.png)

Girin *sınıfı* adı &quot;film&quot;.

Aşağıdaki beş özelliği Ekle `Movie` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Kullanacağız `Movie` filmler veritabanındaki temsil eden sınıf. Her bir örneği bir `Movie` nesne karşılık gelen bir veritabanı tablosu ve her bir özellik içinde bir satıra `Movie` sınıfı tablosunda bir sütun eşleme.

Aynı dosyada, aşağıdaki ekleyin `MovieDBContext` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Sınıfı temsil eder, alma, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanında örnekleri. `MovieDBContext` Türetildiği `DbContext` temel Entity Framework tarafından sağlanan sınıfı.

Başvuru yapabilmek için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Tam *Movie.cs* dosya aşağıda gösterilmektedir. (Çeşitli olmayan deyimleri kullanarak kaldırıldı.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma

`MovieDBContext` Sınıfı, oluşturduğunuz veritabanına bağlanma ve eşleme görevi işler `Movie` veritabanı kayıtlarını nesneleri. Bir soru sorabilirsiniz. ancak, listede bağlantı kurulacak veritabanını belirtmek şeklidir. Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.

Uygulama kökü açın *Web.config* dosya. (Değil *Web.config* dosyası *görünümleri* klasör.) Açık *Web.config* kırmızı renkle dosya.

![](adding-a-model/_static/image2.png)

Aşağıdaki bağlantı dizesi Ekle `<connectionStrings>` öğesinde *Web.config* dosya.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Aşağıdaki örnek bir bölümü gösterilmektedir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Bu küçük kod ve XML temsil eder ve film verileri bir veritabanında saklamak için yazmanız gereken her şeyi miktarıdır.

Ardından, yeni bir oluşturacaksınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmak için kullanabileceğiniz sınıfı.

> [!div class="step-by-step"]
> [Önceki](adding-a-view.md)
> [İleri](accessing-your-models-data-from-a-controller.md)
