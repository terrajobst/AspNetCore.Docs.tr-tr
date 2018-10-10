---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Veritabanının çekirdeğini oluşturma için Code First geçişleri kullanın | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 733b1c343d774e5fa8757808be07a9ae67481d84
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910505"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Veritabanının çekirdeğini oluşturma için Code First Migrations'ı kullanın
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://github.com/MikeWasson/BookService)

Bu bölümde, kullanacağınız [Code First Migrations](https://msdn.microsoft.com/data/jj591621) test verileri ile veritabanının çekirdeğini oluşturma için EF içinde.

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-console[Main](part-3/samples/sample1.cmd)]

Bu komut, projenize geçişleri adlı bir klasör yanı sıra, geçişler klasöründe Configuration.cs adlı bir kod dosyası ekler.

![](part-3/_static/image1.png)

Configuration.cs dosyasını açın. Aşağıdaki **kullanarak** deyimi.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Ardından aşağıdaki kodu ekleyin **Configuration.Seed** yöntemi:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Paket Yöneticisi konsolu penceresinde, aşağıdaki komutları yazın:

[!code-console[Main](part-3/samples/sample4.cmd)]

İlk komut veritabanı oluşturan kodu oluşturur ve ikinci komut, kodu yürütür. Veritabanı kullanılarak yerel olarak oluşturulan [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>(İsteğe bağlı) API'sini keşfedin

Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın. Visual Studio, IIS Express başlar ve web uygulamanızı çalıştırır. Visual Studio bir tarayıcı başlatır ve uygulamanın giriş sayfası açılır.

Visual Studio web projesini çalıştığında, bir bağlantı noktası numarası atar. Aşağıdaki görüntüde, 50524 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

![](part-3/_static/image3.png)

Giriş sayfası, ASP.NET MVC kullanarak uygulanır. Sayfanın üst kısmında "API" ifadesini içeren bir bağlantı yoktur. Bu bağlantıyı web API'si için bir otomatik olarak oluşturulmuş yardım sayfasına getirir. (Bu Yardım sayfasını nasıl oluşturulacağını ve kendi belge sayfasına nasıl ekleyebileceğinizi öğrenmek için bkz: [ASP.NET Web API Yardım sayfaları oluşturma](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) İstek ve yanıt biçimi de dahil olmak üzere bu API hakkında daha fazla ayrıntı görmek için sayfayı bağlantıları hakkında Yardım tıklayabilirsiniz.

![](part-3/_static/image4.png)

API veritabanında CRUD işlemleri sağlar. API özetler.

| Yazarlar |  |
| --- | -- |
| API/yazarlar Al | Tüm yazarları alın. |
| GET API/yazarlar / {id} | Bir yazar kimliğe göre Al |
| POST/api/yazarları | Yeni bir yazar oluşturun. |
| PUT/API'si/yazarlar / {id} | Mevcut bir yazar güncelleştirin. |
| / API'si/yazarlar / {id} Sil | Bir yazar silin. |

| Kitaplar |  |
| --- | -- |
| /Api/Books Al | Tüm Kitaplar alın. |
| Alma/API'si/books / {id} | Bir kitap kimliğe göre Al |
| / Api/books gönderin | Yeni bir kitap oluşturun. |
| PUT/API'si/books / {id} | Mevcut bir kitap güncelleştirin. |
| / API'si/books / {id} Sil | Bir kitap silin. |

## <a name="view-the-database-optional"></a>(İsteğe bağlı) veritabanı görünümü

EF veritabanını Güncelleştir komutu çalıştırdığınızda, veritabanı oluşturulur ve adlı `Seed` yöntemi. Uygulamayı yerel olarak çalıştırdığınızda EF kullanan [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Visual Studio'da veritabanı görüntüleyebilirsiniz. Gelen **görünümü** menüsünde **SQL Server Nesne Gezgini**.

![](part-3/_static/image5.png)

İçinde **sunucuya Bağlan** iletişim, **sunucu adı** düzenleme kutusu, "(localdb) \v11.0" yazın. Bırakın **kimlik doğrulaması** seçeneği olarak "Windows kimlik doğrulaması". **Bağlan**'a tıklayın.

![](part-3/_static/image6.png)

Visual Studio yerel veritabanına bağlanır ve mevcut veritabanlarınızı SQL Server Nesne Gezgini penceresinde gösterir. EF oluşturduğunuz tabloları görmek için düğümü genişletebilirsiniz.

![](part-3/_static/image7.png)

Verileri görüntülemek için bir tablo sağ tıklayıp **görünüm verilerini**.

![](part-3/_static/image8.png)

Aşağıdaki ekran görüntüsünde Books tablosu için sonuçları gösterir. EF veritabanı çekirdek verilerle doldurulmuş ve yazarlar tabloya yabancı anahtar tablosu içeren olduğuna dikkat edin.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Önceki](part-2.md)
> [İleri](part-4.md)
