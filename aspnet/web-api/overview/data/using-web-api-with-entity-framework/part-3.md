---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Veritabanını oluşturmak için Code First geçişleri kullanın | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Veritabanını oluşturmak için Code First geçişleri kullanın
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Bu bölümde, kullanacağınız [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) test verileri ile veritabanı oluşturmak için EF içinde.

Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-console[Main](part-3/samples/sample1.cmd)]

Bu komut, projenize geçişler adlı bir klasör artı geçişler klasöründe Configuration.cs adlı bir kod dosyası ekler.

![](part-3/_static/image1.png)

Configuration.cs dosyasını açın. Aşağıdakileri ekleyin **kullanarak** deyimi.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Ardından aşağıdaki kodu ekleyin **Configuration.Seed** yöntemi:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Paket Yöneticisi konsolu penceresinde aşağıdaki komutları yazın:

[!code-console[Main](part-3/samples/sample4.cmd)]

İlk komut veritabanını oluşturur kod oluşturur ve ikinci komut, kodu yürütür. Veritabanı yerel olarak kullanılarak oluşturulan [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>(İsteğe bağlı) API'sini keşfedin

Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın. Visual Studio IIS Express başlar ve web uygulamanızı çalışır. Visual Studio bir tarayıcı başlatılır ve uygulamanın giriş sayfasını açar.

Visual Studio web projesini çalıştığında, bir bağlantı noktası numarası atar. Aşağıdaki resimde 50524 bağlantı noktası numarasıdır. Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.

![](part-3/_static/image3.png)

Giriş sayfası, ASP.NET MVC kullanılarak uygulanır. Sayfanın en üstünde "API" belirten bir bağlantı yoktur. Bu bağlantıyı web API'si için otomatik olarak oluşturulan yardım sayfasına getirir. (Bu Yardım sayfası nasıl oluşturulacağını ve kendi belgeleri sayfasına nasıl ekleyebileceğiniz öğrenmek için bkz: [ASP.NET Web API Yardım sayfaları oluşturma](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) İstek ve yanıt biçimi dahil olmak üzere API, ilgili ayrıntıları görmek için sayfa bağlantıları hakkında Yardım tıklatabilirsiniz.

![](part-3/_static/image4.png)

API CRUD işlemleri veritabanında sağlar. API özetler.

| Yazarlar |  |
| --- | -- |
| API/yazarlar Al | Tüm yazarları alın. |
| GET API/yazarlar / {id} | Bir yazar tarafından kimliği alma |
| POST/api/yazarlar | Yeni bir yazar oluşturun. |
| PUT /api/yazarlar / {id} | Varolan bir yazar güncelleştirin. |
| /Api/yazarlar / {id} Sil | Bir yazar silin. |

| Kitaplar |  |
| --- | -- |
| /Api/Books Al | Tüm books alın. |
| /Api/books / {id} Al | Kimliğe göre defteri alma |
| / Api/books sonrası | Yeni bir rehberi oluşturun. |
| PUT /api/books / {id} | Varolan bir kitabı güncelleştirin. |
| /Api/books / {id} Sil | Kitap silin. |

## <a name="view-the-database-optional"></a>Görünüm veritabanı (isteğe bağlı)

Update-Database komutunu çalıştırdığınızda EF veritabanı oluşturulur ve adlı `Seed` yöntemi. Uygulamayı yerel olarak çalıştırdığınızda EF kullanan [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Visual Studio'da veritabanı görüntüleyebilirsiniz. Gelen **Görünüm** menüsünde, select **SQL Server Nesne Gezgini**.

![](part-3/_static/image5.png)

İçinde **sunucuya Bağlan** iletişim, **sunucu adı** düzenleme kutusu, "(localdb) \v11.0" yazın. Bırakın **kimlik doğrulaması** seçeneği "Windows kimlik doğrulaması" olarak. **Bağlan**'a tıklayın.

![](part-3/_static/image6.png)

Visual Studio yerel veritabanına bağlanır ve var olan veritabanlarını SQL Server Nesne Gezgini penceresinde gösterir. EF oluşturulan tabloları görmek için düğümleri genişletebilirsiniz.

![](part-3/_static/image7.png)

Verileri görüntülemek için bir tabloyu sağ tıklatın ve seçin **görünüm verilerini**.

![](part-3/_static/image8.png)

Aşağıdaki ekran görüntüsünde Books tablosu için sonuçları gösterir. EF veritabanı çekirdek verileri ile doldurulur ve yazarlar tablosuna yabancı anahtar tablosu içerir dikkat edin.

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[Önceki](part-2.md)
[sonraki](part-4.md)
