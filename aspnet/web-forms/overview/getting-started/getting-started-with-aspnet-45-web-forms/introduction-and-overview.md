---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.7 Web Forms ve Visual Studio 2017 ile çalışmaya başlama | Microsoft Docs
author: Erikre
description: Bu adım adım öğretici serisinin ASP.NET 4.7 ve Microsoft Visual Studio kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri sağlanır.
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341559"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>ASP.NET 4.5 Web Forms ve Visual Studio 2017 ile çalışmaya başlama
====================

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Bu öğretici serisinde, ASP.NET 4.5 ve Microsoft Visual Studio 2017 ile bir ASP.NET Web Forms uygulaması oluşturma işlemini göstermektedir. 

## <a name="introduction"></a>Giriş

Bu öğretici serisinde, Visual Studio 2017 ve ASP.NET 4.5 kullanarak bir ASP.NET Web Forms uygulaması oluşturma işleminde size yol gösterir. Adlı bir uygulama oluşturacaksınız **Wingtip Toys** - çevrimiçi öğe satış basitleştirilmiş bir mağaza web sitesi. Serinin sırasında yeni ASP.NET 4.5 özellikleri vurgulanır.

### <a name="target-audience"></a>Hedef kitle

ASP.NET Web formları için yeni geliştiricilerin, Bu öğretici serisinin hedef kitlesi önerilir.

Aşağıdaki alanlarda bazı bilgisine sahip olmalıdır:

- Nesne odaklı programlama (OOP) ve dilleri
- Web geliştirme (HTML, CSS, JavaScript)
- İlişkisel veritabanları
- N katmanlı mimari

Bu alanlar gözden geçirmek için aşağıdaki içeriği Çincesi göz önünde bulundurun:

- [Visual C# kullanmaya Başlarken](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web geliştirme](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [İlişkisel veritabanı](http://en.wikipedia.org/wiki/Relational_database)
- [Çok katmanlı mimari](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Uygulama özellikleri

Bu seride sunulan ASP.NET Web formu özellikler şunlardır:

- Web uygulama projesi (Web sitesi projesi değil)
- Web Forms
- Ana sayfalar, yapılandırma
- Önyükleme
- Entity Framework Code ilk olarak LocalDB
- İsteği doğrulama
- Kesin türü belirtilmiş veri denetimleri
- Model bağlama
- Veri ek açıklamaları
- Değer sağlayıcıları
- SSL ve OAuth
- ASP.NET Identity, yapılandırma ve yetkilendirme
- Örtük doğrulama
- Yönlendirme
- ASP.NET Hata İşleme

### <a name="application-scenarios-and-tasks"></a>Uygulama senaryolar ve görevler

Öğretici serisinin görevler aşağıdakileri içerir:

- Çalıştıran yeni bir proje oluşturma ve gözden geçirme
- Veritabanı yapısı oluşturma
- Başlatma ve bir veritabanı dengeli dağıtım
- Stilleri, grafik ve bir ana sayfa ile kullanıcı arabirimini özelleştirme
- Sayfalar ve gezinti ekleme
- Menü ayrıntılarını ve ürün verileri görüntüleme
- Bir alışveriş sepeti oluşturmak
- Ekleme SSL ve OAuth desteği
- Bir ödeme yöntemi ekleme
- Bir yönetici rolü ve bir kullanıcı uygulamaya dahil
- Belirli bir sayfa ve klasörüne erişimi kısıtlama
- Web uygulaması için bir dosya karşıya yükleme
- Giriş Doğrulaması uygulama
- Web uygulama için rota kaydediliyor
- Uygulama hata işleme ve hata günlüğü

## <a name="overview"></a>Genel Bakış

Bu öğretici serisinin programlama kavramları bildiğiniz birisi yöneliktir, ancak yeni ASP.NET Web formları için değildir. Zaten ASP.NET Web Forms ile biliyorsanız, bu seri hala yeni ASP.NET 4.5 özellikleri hakkında bilgi edinmenize yardımcı olabilir. Sağlanan ek Web Forms öğreticiler için bkz. okuyucular programlama kavramları ve ASP.NET Web Forms ile bilinmeyen [Başlarken](../../../index.md) ASP.NET Web sitesinde bölümü.

Bu öğretici serisinde sağlanan ASP.NET 4.5 aşağıdaki özellikleri içerir:

- Sunan projeleri oluşturmak için basit bir kullanıcı Arabirimiyle [birçok ASP.NET çerçeveler için destek](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web formları, MVC ve Web API'si).
- [Önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), düzen, tema oluşturma ve duyarlı tasarım framework.
- [ASP.NET Identity](../../../../identity/index.md), tüm ASP.NET çerçeveleri ve çalışan aynı web barındırma yazılımı dışında IIS ile çalışan yeni bir ASP.NET üyelik sistemi.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Bir güncelleştirme sağlayarak Entity Framework için:
  - Alma ve kesin tür belirtilmiş nesneler olarak verilerini işleme
  - Zaman uyumsuz olarak veri erişimi
  - Geçici bağlantı hataları işleme
  - Günlük SQL deyimleri

Tam ASP.NET 4.5 özellik listesi için bkz: [için ASP.NET and Web Tools Visual Studio 2013 sürüm notları](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys örnek uygulaması

Aşağıdaki ekran görüntüleri, Bu öğretici serisinde oluşturduğunuz ASP.NET Web Forms uygulaması arasındadır. Visual Studio'da uygulamayı çalıştırdığınızda, aşağıdaki web giriş sayfası görüntülenir.

![Wingtip Toys - varsayılan sayfası](introduction-and-overview/_static/image1.png)

Yeni bir kullanıcı olarak Kaydol veya var olan bir kullanıcı olarak oturum açın. Üst gezinti, ürün kategorilerini ve bunların ürünlerini veritabanından bağlantılar içerir.

Seçerseniz **ürünleri**, kullanılabilir tüm ürünleri görüntülenir. 

![Wingtip Toys - ürünleri](introduction-and-overview/_static/image2.png)

Belirli bir ürün seçerseniz, ürün ayrıntıları görüntülenir.


![Wingtip Toys - Ürün Ayrıntıları](introduction-and-overview/_static/image3.png)

Bir kullanıcı olarak kaydedin ve Web Forms şablonu varsayılan işlevsellik bilgilerinizle oturum açın. Bu öğreticide, ayrıca var olan bir Gmail hesabını kullanarak oturum açmanız nasıl açıklar. Ayrıca, eklemek ve ürün veritabanından kaldırmak için yönetici olarak oturum açabilirsiniz.

![Wingtip Toys - oturum açma](introduction-and-overview/_static/image4.png)

Bir kullanıcı olarak açtıktan sonra alışveriş sepeti ve PayPal ile kasa işlemleri ürünleri ekleyebilirsiniz. Örnek uygulama, PayPal'ın Geliştirici korumalı alanında çalışmak üzere tasarlanmıştır. Hiçbir gerçek parayı işlem gerçekleşir.

![Wingtip Toys - alışveriş sepeti](introduction-and-overview/_static/image5.png)

PayPal, hesap, siparişi ve Ödeme bilgilerinizi onaylar.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

PayPal ' iade edildikten sonra gözden geçirin ve siparişinizi tamamlayın.

![Wingtip Toys - siparişi gözden geçirme](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki yazılımlar bilgisayarınızda yüklü olduğundan emin olun:

- [Microsoft Visual Studio 2017 veya Microsoft Visual Studio Community 2017'yi](https://visualstudio.microsoft.com/downloads/).

.NET Framework otomatik olarak yüklenir.

Bu öğretici serisinde, Microsoft Visual Studio Community 2017 kullanır. Ya da bu hesabı kullanabilirsiniz veya Microsoft Visual Studio 2017 Bu öğretici serisinin tamamlanmasını.

Visual Studio hakkında aşağıdakileri unutmayın:

* Microsoft Visual Studio 2017 ve Microsoft Visual Studio Community 2017 denir *Visual Studio* Bu öğretici serisinin boyunca.

* Visual Studio 2017, herhangi bir eski sürümü zaten yüklü yanındaki yüklenir. Önceki sürümlerinde oluşturulan siteleri, Visual Studio 2017'de açılabilir ve önceki sürümlerde açmak devam edin.

* İlk kez Visual Studio'yu başlattığınız, seçtiğiniz varsayılır *Web geliştirme* ayarları. Daha fazla bilgi için [nasıl yapılır: Web geliştirme ortam ayarlarını Seç](https://msdn.microsoft.com/library/ff521558.aspx).

Önkoşullar yüklendikten sonra Bu öğretici serisinde sunulan Web projesi oluşturmaya başlamak hazırsınız demektir.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin

 Tamamlanan örnek applicatiion adresindeki MSDN Örnekler sitesinden dilediğiniz zaman indirebilirsiniz:

[ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Bu indirme, aşağıdaki öğeleri içerir:

- Örnek uygulamada *WingtipToys* klasör.
- Örnek uygulama oluşturmak için kullanılan kaynakları *WingtipToys varlıklar* klasöründe *WingtipToys* klasör.

İndirme bir *.zip* dosya. Bu öğretici serisinin oluşturan projeyi bulun ve seçin görmek için *C#* klasöründeki .zip dosyası. Kaydet C# Visual Studio projeleriyle çalışmak için klasörü klasörü. Varsayılan olarak, Visual Studio 2017 projeleri klasördür:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong>

Yeniden adlandırma ***C#*** klasörüne ***WingtipToys***.

> [!NOTE]
> Adlı bir klasör zaten varsa *WingtipToys* projeler klasörünüze geçici olarak var olan bir klasörü yeniden adlandırmadan önce Yeniden Adlandır *C#* klasörüne *WingtipToys*.

Projeyi çalıştırmak için *WingtipToys* klasörü ve çift *WingtipToys.sln* dosya. Visual Studio 2017, projeyi açar. Ardından, sağ *Default.aspx* dosyası **Çözüm Gezgini** seçip **tarayıcıda görüntüle**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Bir ASP.NET Web Forms sınava içeriği gözden geçirmek için

Öğretici serisinin tamamladıktan sonra bilginizi test ve önemli kavramları güçlendiren bir testi uygulayın. Bütün soruların yanıtını bir açıklama ve ek yönergeler için bağlantılar sağlar.

 * [Test ASP.NET Web formları](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Öğretici desteği ve açıklamalar

Sorularınız ve Yorumlarınız için dahil soru cevap bölümünde kullanın [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) örnek sayfası.

Bu öğretici serisinin üzerine yorum davetlidir. Bu öğretici serisinin güncelleştirildiğinde, her türlü çabayı düzeltmeleri veya geliştirme önerileri göz önünde bulundurun yapılır.

Bir hata, ilgili hata iletilerini karıştırıcı olabilir oluyorsa sorunu gidermek nasıl iyi bir açıklama ile. Yardım için kontrol edebilirsiniz [ASP.NET forumları](https://forums.asp.net/). Başka bir iyi soru cevap bölümünde kaynağıdır [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) örnek sayfası. 

> [!div class="step-by-step"]
> [Next](create-the-project.md)
