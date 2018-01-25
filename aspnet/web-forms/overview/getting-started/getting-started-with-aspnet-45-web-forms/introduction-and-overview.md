---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "ASP.NET 4.5 Web formları ve Visual Studio 2013 ile çalışmaya başlama | Microsoft Docs"
author: Erikre
description: "Bu adım adım öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>ASP.NET 4.5 Web formları ve Visual Studio 2013 ile çalışmaya başlama
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu adım adım öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. [Test ASP.NET Web formları](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Bilginiz test edin ve ASP.NET Web Forms test gerçekleştirerek temel kavramları pekiştirmek. Bu test, Bu öğretici serisinde bulunan içerikten özel olarak tasarlanmıştır. Test her soruyu ek yönergelere bağlantılar ile birlikte bir açıklama sağlar.


## <a name="introduction"></a>Giriş

Bu öğreticiler dizi Web ve ASP.NET 4.5 için Visual Studio Express 2013 kullanarak bir ASP.NET Web Forms uygulama oluşturmak için gereken adımlarda size kılavuzluk eder.

Oluşturacaksınız uygulama adlı **Wingtip Toys**. Çevrimiçi öğeleri sattığı deposu ön web sitesi basitleştirilmiş bir örnektir. Bu öğretici seri ASP.NET 4.5 içinde kullanılabilen yeni özellikleri vurgular.

Yorumlar Hoş Geldiniz ve, öneriler temelinde Bu öğretici seri güncelleştirmek için her türlü çabayı hale getireceğiz.

### <a name="download-completed-project"></a>Yükleme Tamamlandı proje

Tamamlanan öğretici içeren bir C# projesi indirebilirsiniz.

- [ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys Başlarken](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>İlgili ASP.NET Web Forms test gerçekleştirerek içeriği gözden geçir

Bu öğreticiyi tamamladıktan sonra bilginizi sınayın ve temel kavramları gerçekleştirerek pekiştirmek [ASP.NET Web Forms test](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Bu test, Bu öğretici serisinde bulunan içerikten özel olarak tasarlanmıştır. Test her soruyu ek yönergelere bağlantılar ile birlikte bir açıklama sağlar.

- [Test ASP.NET Web formları](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Hedef Kitle

Bu öğretici dizisinin hedef kitle ASP.NET Web formları için yeni olan deneyimli geliştiriciler ' dir. Bu öğretici serisinde ilgilenen bir geliştirici, aşağıdaki yetenekleri olmalıdır:

- Tanıdık bir nesne odaklı programlama (OOP) dil
- Tanıdık Web geliştirme kavramları (HTML, CSS, JavaScript)
- İlişkisel veritabanı kavramları hakkında bilgi sahibi
- N katmanlı mimari kavramlarını aşina

Yukarıda listelenen alanlarını gözden geçirme ilgileniyorsanız, aşağıdaki içeriği gözden geçirme göz önünde bulundurun:

- [Visual C# kullanmaya Başlarken](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web geliştirme](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [İlişkisel veritabanı](http://en.wikipedia.org/wiki/Relational_database)
- [Çok katmanlı mimarisi](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Uygulama özellikleri

Bu serideki sunulan ASP.NET Web formu özellikleri içerir:

- Web uygulaması projesi (Web sitesi projesini değil)
- Web Forms
- Ana sayfalar, yapılandırma
- önyükleme
- Entity Framework kod ilk olarak, yerel veritabanı
- İstek doğrulama
- Bağlama, veri ek açıklamaları Model ve sağlayıcıları değer kesin türü belirtilmiş veri denetimleri
- SSL ve OAuth
- ASP.NET kimliği, yapılandırma ve yetkilendirme
- Örtük doğrulama
- Yönlendirme
- ASP.NET hata işleme

### <a name="application-scenarios-and-tasks"></a>Uygulama senaryolar ve görevler

Bu serideki gösterilen görevler aşağıdakileri içerir:

- Oluşturma, gözden geçirme ve yeni projesi çalıştırma
- Veritabanı yapısını oluşturma
- Başlatma ve veritabanı üretme
- Stiller, grafik ve bir ana sayfa kullanarak kullanıcı Arabirimi özelleştirme
- Sayfalar ve gezinti ekleme
- Menü ayrıntıları ve ürün verileri görüntüleme
- Alışveriş sepeti oluşturma
- Ekleme SSL ve OAuth desteği
- Bir ödeme yöntemi ekleme
- Bir yönetici rolü ve bir kullanıcı uygulama da dahil olmak üzere
- Belirli sayfalara ve klasörüne erişimi kısıtlama
- Web uygulaması için bir dosya karşıya yükleme
- Giriş Doğrulaması uygulama
- Web uygulaması için yolları kaydediliyor
- Hata işleme ve hata günlüğünü uygulama

## <a name="overview"></a>Genel Bakış

ASP.NET Web formları için yeniyseniz sağ öğretici sahip, ancak programlama kavramları ile benzerlik sahip. ASP.NET Web Forms bilginiz varsa, ASP.NET 4.5 içinde kullanılabilen yeni özellikleri tarafından Bu öğretici serisinde yararlı olabilir. Programlama kavramları ve ASP.NET Web Forms bilginiz yoksa Web formları sağlanan ek öğreticilere bakın [Başlarken](../../../index.md) bölümü ASP.NET Web sitesinde.

Belirli **son** ASP.NET 4.5 özellikleri sağlanan bu Web Forms öğretici serisi şunları içerir:

- Bu teklif projeleri oluşturmak için basit bir kullanıcı Arabirimi [desteklemek için birden çok ASP.NET çerçeveyi](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC ve Web API).
- [Önyükleme](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), esnek tasarım ve tema özellikleri sağlayan bir düzen ve tema framework.
- [ASP.NET Identity](../../../../identity/index.md), tüm ASP.NET çerçeveler ve çalışan aynı web barındırma IIS dışında yazılım ile çalışan yeni bir ASP.NET üyelik sistemi.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), bir güncelleştirme almak ve veri türü kesin olarak değiştirmek sağlayan Entity Framework nesneleri yazılan, erişim verilerini zaman uyumsuz olarak geçici bağlantı hataları işlemek ve oturum SQL deyimleri.

ASP.NET 4.5 özelliklerin tam listesi için bkz: [ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları için](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys örnek uygulama

Aşağıdaki ekran görüntüleri, Bu öğretici serisinde oluşturacak ASP.NET Web forms uygulaması hızlı bir görünümünü sağlar. Uygulama için Visual Studio Express 2013 Web çalıştırdığınızda, aşağıdaki web giriş sayfasını görürsünüz.

![Wingtip Toys - varsayılan sayfa](introduction-and-overview/_static/image1.png)

Yeni bir kullanıcı olarak Kaydol veya varolan bir kullanıcı oturum açın. Gezinti en üstünde mevcut ürünler veritabanından alarak her ürün kategorisi için sağlanır.

Ürünler bağlantısını seçerek, tüm kullanılabilir ürünlerin listesini görmeye olacaktır.

![Wingtip Toys - ürünler](introduction-and-overview/_static/image2.png)

Bireysel Ürün Ayrıntıları listelenen ürünlerden herhangi birini seçerek de görebilirsiniz.

![Wingtip Toys - Ürün Ayrıntıları](introduction-and-overview/_static/image3.png)

Bir kullanıcı olarak kaydedebilir ve Web Forms şablonu varsayılan işlevselliğini kullanarak oturum açın. Bu öğretici aynı zamanda var olan bir Gmail hesabını kullanarak oturum açma açıklanmaktadır. Ayrıca, eklemek ve ürünleri veritabanından kaldırmak için yönetici olarak oturum açabilirsiniz.

![Wingtip Toys - oturum açma](introduction-and-overview/_static/image4.png)

Bir kullanıcı olarak oturum açtıktan sonra alışveriş sepeti ve checkout PayPal ile ürünleri ekleyebilirsiniz. Bu örnek uygulama PayPal'ın Geliştirici sandbox ile çalışacak biçimde tasarlanmıştır unutmayın. Hiç gerçek para işlem gerçekleşir.

![Wingtip Toys - alışveriş sepeti](introduction-and-overview/_static/image5.png)

PayPal hesabı, sipariş ve ödeme bilgileri onaylar.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

PayPal döndüğünüzde gözden geçirin ve siparişinizin tamamlayın.

![Wingtip Toys - sipariş gözden geçirme](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki yazılım bilgisayarınızda yüklü olduğundan emin olun:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework otomatik olarak yüklenir.

Bu öğretici seri Web için Microsoft Visual Studio Express 2013 kullanır. Bu öğretici seri tamamlamak için Web için Visual Studio Express 2013 Microsoft veya Microsoft Visual Studio 2013'nı kullanabilirsiniz.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici seri anılacaktır.


Yüklü Visual Studio sürümü zaten varsa, yükleme işlemi yanındaki mevcut sürümünü Visual Studio 2013 veya Web için Visual Studio Express 2013 Microsoft yükleyin. Önceki sürümlerde oluşturan siteleri Visual Studio 2013'te açılabilir ve önceki sürümlerde açmak devam edin.

> [!NOTE] 
> 
> Bu kılavuz, seçtiğiniz varsayar *Web geliştirme* ayarlar koleksiyonu, Visual Studio ilk başlattığınızda. Daha fazla bilgi için bkz: [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin

Önkoşulları yüklendikten sonra Bu öğretici serisinde sunulan yeni Web projesi oluşturmaya başlamak hazırsınız demektir. Başlamayı tercih ederseniz **isteğe bağlı olarak** Bu öğretici seri oluşturur örnek uygulamayı çalıştırın, MSDN örnekleri sitesinden yükleyebilirsiniz. Bu yükleme aşağıdakileri içerir:

- Örnek uygulama *WingtipToys* klasör.
- Örnek uygulama oluşturmak için kullanılan kaynakları *WingtipToys varlıklar* klasöründe *WingtipToys* klasör.

#### <a name="download-the-file-from-msdn-samples-site"></a>Dosya MSDN örnekleri sitesinden yükleyin:

[ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys Başlarken](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

Karşıdan yüklemenin bir *.zip* dosyası. Bu öğretici seri oluşturur projeyi bulun ve seçin, görmek için *C#*klasöründe *.zip* dosyası. Kaydet *C#* Folder Visual Studio 2013 projelerle çalışmak için kullandığınız klasör. Varsayılan olarak, Visual Studio 2013 projeleri klasör aşağıda verilmiştir:

**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**

Yeniden Adlandır ***C#*** klasörüne ***WingtipToys***.

> [!NOTE]
> Adlı bir klasör zaten varsa *WingtipToys* projeleri klasörünüzde geçici olarak bu varolan bir klasörü yeniden adlandırmadan önce yeniden adlandırın *C#* klasörüne *WingtipToys*.


Projeyi çalıştırmak için *WingtipToys* klasörü ve çift *WingtipToys.sln* dosya. Visual Studio 2013 proje açılır. Ardından, sağ *Default.aspx* dosya Çözüm Gezgini penceresinde ve sağ tıklatma menüsünden tarayıcıda görüntüle'yi tıklatın.

### <a name="tutorial-support-and-comments"></a>Eğitmen desteği ve açıklamaları

İle birlikte gelen Q ve bir bölümün [ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) herhangi bir sorunuz veya açıklamalar için örnek (C#).

Bu öğretici serilerinde açıklamaları Hoş Geldiniz ve hesap düzeltmeleri veya öğretici açıklamalarda sağlanan geliştirmeleri önerileri almak için Bu öğretici seri güncelleştirildiğinde her türlü çabayı yapılır.

Geliştirme sırasında bir hata olduğunda ya da Web sitesinin düzgün çalışmıyorsa, hata iletileri karmaşık ipuçları sorun kaynağına verebilir veya bu sorunun nasıl açıklayabilir değil. Bazı yaygın sorun senaryolar ile yardımcı olmak için de kullanabilirsiniz [ASP.NET forumları](https://forums.asp.net/) veya birlikte Q ve bir bölüm [ASP.NET 4.5 Web formları ve Visual Studio 2013 - Wingtip Toys ile çalışmaya başlama](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) örnek. Bir hata iletisi alırsınız veya öğreticileri devam ederken bir sorun oluşması, yukarıdaki konumları denetlediğinizden emin olun.

>[!div class="step-by-step"]
[Next](create-the-project.md)
