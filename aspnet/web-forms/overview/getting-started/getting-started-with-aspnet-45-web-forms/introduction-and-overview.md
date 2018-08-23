---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.5 Web Forms ve Visual Studio 2013 ile çalışmaya başlama | Microsoft Docs
author: Erikre
description: Bu adım adım öğretici serisinin ASP.NET 4.5 ve Microsoft Visual Studio ifade etme kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: ad5e97cd596e146f742c4c5e882d3938005070d1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753797"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>ASP.NET 4.5 Web Forms ve Visual Studio 2013 ile çalışmaya başlama
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu adım adım öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. [Test ASP.NET Web formları](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Bilginizi test edin ve ASP.NET Web Forms test gerçekleştirerek önemli kavramları güçlendiren. Bu test, Bu öğretici serisinde bulunan içeriğinden özel olarak tasarlanmıştır. Testteki her soru olduğuyla ilgili ek yönergeler için bağlantılar sağlar.


## <a name="introduction"></a>Giriş

Bu öğretici serisinde, Web uygulamaları ve ASP.NET 4.5 için Visual Studio Express 2013 kullanarak bir ASP.NET Web Forms uygulaması oluşturmak için gereken adımlarda size kılavuzluk eder.

Oluşturduğunuz uygulama adlı **Wingtip Toys**. Çevrimiçi öğe satan bir ön mağazası web sitesine basitleştirilmiş bir örneğidir. Bu öğretici serisinde, ASP.NET 4.5 içinde kullanılabilen yeni özellikleri vurgular.

Açıklamalar davetlidir ve Bu öğretici serisinde, öneriler temelinde güncelleştirmek için her türlü çabayı oluşturacağız.

### <a name="download-completed-project"></a>Proje indirme tamamlandı

Tamamlanan öğretici içeren bir C# projesi indirebilirsiniz.

- [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>İlgili ASP.NET Web Forms test gerçekleştirerek içeriği gözden geçir

Bu öğreticiyi tamamladıktan sonra bilginizi test ve yararlanarak önemli kavramları güçlendiren [ASP.NET Web Forms test](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Bu test, Bu öğretici serisinde bulunan içeriğinden özel olarak tasarlanmıştır. Testteki her soru olduğuyla ilgili ek yönergeler için bağlantılar sağlar.

- [Test ASP.NET Web formları](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Hedef Kitle

Bu öğretici serisinin hedef kitlesi, ASP.NET Web formları için yeni deneyimli geliştiriciler olur. Bu öğretici serisinde isteyen bir geliştirici, aşağıdaki yetenekleri sahip olmanız gerekir:

- Tanıdık bir nesne yönelimli programlama (OOP) dil
- Tanıdık, Web geliştirme kavramları (HTML, CSS, JavaScript)
- İlişkisel Veritabanı kavramlarına aşina
- N katmanlı mimari kavramları hakkında bilgi sahibi

Yukarıda listelenen alanları gözden geçirme içinde ilgileniyorsanız aşağıdaki içeriği gözden geçirme göz önünde bulundurun:

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
- Kesin türü belirtilmiş veri denetimleri, Model, veri ek açıklamaları, bağlama ve değer sağlayıcıları
- SSL ve OAuth
- ASP.NET Identity, yapılandırma ve yetkilendirme
- Örtük doğrulama
- Yönlendirme
- ASP.NET hata işleme

### <a name="application-scenarios-and-tasks"></a>Uygulama senaryolar ve görevler

Bu seride gösterilen görevler aşağıdakileri içerir:

- Oluşturma, gözden geçirme ve yeni proje çalıştırma
- Veritabanı yapısı oluşturma
- Başlatma ve veritabanında dengeli dağıtım
- Stilleri, grafik ve bir ana sayfa kullanarak kullanıcı arabirimini özelleştirme
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

ASP.NET Web formları için yeni başladıysanız ancak programlama kavramları ile aşinalık yoksa, doğru öğretici sahip. ASP.NET Web Forms ile bilginiz varsa, Bu öğretici serisine ASP.NET 4.5 içinde kullanılabilen yeni özellikler yararlı olabilir. Web formları içinde sağlanan ek öğreticiler programlama kavramları ve ASP.NET Web Forms ile bilginiz yoksa bkz [Başlarken](../../../index.md) ASP.NET Web sitesinde bölümü.

Belirli **son** ASP.NET 4.5 özellikleri koşuluyla bu Web formlarında öğretici serisinin şunları içerir:

- Bu teklifi oluşturmak için basit bir kullanıcı Arabirimi projeleri [desteklemek için birden çok ASP.NET çerçeve](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web formları, MVC ve Web API'si).
- [Önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), duyarlı tasarım ve Tema oluşturma özellikleri sağlayan bir düzen ve Tema oluşturma çerçevesi.
- [ASP.NET Identity](../../../../identity/index.md), tüm ASP.NET çerçeveleri ve çalışan aynı web barındırma yazılımı dışında IIS ile çalışan yeni bir ASP.NET üyelik sistemi.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), nesneleri almak ve veri türü kesin olarak işlemek sağlayan Entity Framework için bir güncelleştirme yazılan, verilere zaman uyumsuz olarak geçici bağlantı hataları işlemek ve oturum SQL deyimleri.

ASP.NET 4.5 özelliklerin tam bir listesi için bkz. [için ASP.NET and Web Tools Visual Studio 2013 sürüm notları](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys örnek uygulaması

Aşağıdaki ekran görüntüleri, Bu öğretici serisinde oluşturacağınız ASP.NET Web forms uygulaması hızlı bir görünümünü sağlar. Visual Studio Express 2013 Web için gelen uygulamayı çalıştırdığınızda, aşağıdaki web giriş sayfasını görürsünüz.

![Wingtip Toys - varsayılan sayfası](introduction-and-overview/_static/image1.png)

Yeni bir kullanıcı olarak Kaydol veya var olan bir kullanıcı olarak oturum açın. Gezinti üst kısmında, veritabanından çalıştırarak kullanılabilir ürünlere alarak her ürün kategorisi için sağlanır.

Ürünleri bağlantıyı seçerek kullanılabilir tüm ürünlerin listesini görmek mümkün olacaktır.

![Wingtip Toys - ürünleri](introduction-and-overview/_static/image2.png)

Ayrıca, listelenen ürünlerden birini seçerek tek tek ürün ayrıntıları görebilirsiniz.

![Wingtip Toys - Ürün Ayrıntıları](introduction-and-overview/_static/image3.png)

Bir kullanıcı olarak kaydedin ve Web Forms şablonu varsayılan işlevlerini kullanarak oturum açın. Bu öğreticide, ayrıca var olan bir Gmail hesabını kullanarak oturum nasıl açıklar. Ayrıca, eklemek ve ürün veritabanından kaldırmak için yönetici olarak oturum açabilirsiniz.

![Wingtip Toys - oturum açma](introduction-and-overview/_static/image4.png)

Bir kullanıcı olarak oturum açtıktan sonra alışveriş sepeti ve PayPal ile kasa işlemleri ürünleri ekleyebilirsiniz. Bu örnek uygulama PayPal'ın Geliştirici sanal işlev görecek şekilde tasarlanmış olduğunu unutmayın. Hiçbir gerçek parayı işlem gerçekleşir.

![Wingtip Toys - alışveriş sepeti](introduction-and-overview/_static/image5.png)

PayPal, hesap, siparişi ve Ödeme bilgilerinizi onaylar.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

PayPal ' iade edildikten sonra gözden geçirin ve siparişinizi tamamlayın.

![Wingtip Toys - siparişi gözden geçirme](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki yazılımlar bilgisayarınızda yüklü olduğundan emin olun:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework otomatik olarak yüklenir.

Bu öğretici serisinde, Web için Microsoft Visual Studio Express 2013 kullanır. Bu öğretici serisinin tamamlanmasını Web için Visual Studio Express 2013 Microsoft veya Microsoft Visual Studio 2013'ü kullanabilirsiniz.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici serisinin denir.


Bir Visual Studio sürümü zaten varsa, yükleme işlemi yanındaki mevcut sürümü Visual Studio 2013 veya Web için Visual Studio Express 2013 Microsoft yükleyin. Önceki sürümlerinde oluşturulan siteleri, Visual Studio 2013'te açılabilir ve önceki sürümlerde açmak devam edin.

> [!NOTE] 
> 
> Bu kılavuzda, seçtiğiniz varsayılır *Web geliştirme* ayarlar koleksiyonu, Visual Studio'yu ilk başlattığınızda. Daha fazla bilgi için [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin

Önkoşullar yüklendikten sonra Bu öğretici serisinde sunulan yeni Web projesi oluşturmaya başlamak hazır olursunuz. İsteyip istemediğini **isteğe bağlı olarak** oluşturan Bu öğretici serisinin örnek uygulamayı çalıştırın, MSDN Örnekler sitesinden indirebilirsiniz. Bu indirme, aşağıdakileri içerir:

- Örnek uygulamada *WingtipToys* klasör.
- Örnek uygulama oluşturmak için kullanılan kaynakları *WingtipToys varlıklar* klasöründe *WingtipToys* klasör.

#### <a name="download-the-file-from-msdn-samples-site"></a>MSDN Örnekler siteden dosya indirme:

[ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

İndirme bir <em>.zip</em> dosya. Bu öğretici serisinin oluşturan projeyi bulun ve seçin görmek için <em>C#</em>klasöründe <em>.zip</em> dosya. Kaydet <em>C#</em> Folder Visual Studio 2013 projeleriyle çalışmak için klasör. Varsayılan olarak, Visual Studio 2013 projeler klasöründe aşağıda verilmiştir:

<strong>C:\Users&#92;</strong><strong><em>&lt;kullanıcıadı&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

Yeniden adlandırma ***C#*** klasörüne ***WingtipToys***.

> [!NOTE]
> Adlı bir klasör zaten varsa *WingtipToys* projeler klasörünüze geçici olarak var olan bir klasörü yeniden adlandırmadan önce Yeniden Adlandır *C#* klasörüne *WingtipToys*.


Projeyi çalıştırmak için *WingtipToys* klasörü ve çift *WingtipToys.sln* dosya. Visual Studio 2013 proje açılır. Ardından, sağ *Default.aspx* dosya Çözüm Gezgini penceresinde ve sağ tıklama menüsünden tarayıcıda görüntüle'ye tıklayın.

### <a name="tutorial-support-and-comments"></a>Öğretici desteği ve açıklamalar

Bulunan soru cevap bölümünde kullanın [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) soru ya da Yorumlarınız için örneği (C#).

Bu öğretici serisinin üzerine yorum davetlidir ve hesap düzeltmeleri veya öğretici yorumlar bölümünde sağlanan geliştirmeleri için öneriler almak için Bu öğretici serisinin güncelleştirildiğinde her türlü çabayı yapılacaktır.

Geliştirme sırasında bir hata meydana geldiğinde veya Web sitesi düzgün çalışmıyorsa, hata iletileri karmaşık nedene sorun kaynağına verebilir veya bu sorunun nasıl açıklayabilir değil. Bazı yaygın sorun senaryoları ile yardımcı olmak için de kullanabilirsiniz [ASP.NET forumları](https://forums.asp.net/) veya bulunan soru cevap bölümünde [ASP.NET 4.5 Web Forms ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) örnek. Şu öğreticileri gibi bir sorun oluşması veya bir hata iletisi alırsanız, yukarıdaki konumları kontrol etmeyi unutmayın.

> [!div class="step-by-step"]
> [Next](create-the-project.md)
