---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET MVC genel bakış | Microsoft Docs
author: microsoft
description: ASP.NET MVC uygulaması ile ASP.NET Web Forms uygulamaları arasındaki farklar hakkında bilgi edinin. Bir ASP.NET MVC uygulaması derleme karar vermenize öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26564567"
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC genel bakış
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC uygulaması ile ASP.NET Web Forms uygulamaları arasındaki farklar hakkında bilgi edinin. Bir ASP.NET MVC uygulaması derleme karar vermenize öğrenin.


Model-View-Controller (MVC) tasarım örüntüsü uygulamanın üç ana bileşene ayırır: model, Görünüm ve denetleyici. ASP.NET MVC çerçevesi, MVC tabanlı Web uygulamaları oluşturmak için ASP.NET Web Forms örüntüsüne bir alternatif sunar. ASP.NET MVC çerçevesi bir basit, yüksek düzeyde sınanabilir bir sunu çerçevesidir, (Web Forms tabanlı uygulamalar olduğu gibi) ana sayfalar ve üyelik tabanlı kimlik doğrulaması gibi mevcut ASP.NET özellikleriyle tümleşiktir. MVC çerçevesi tanımlanan **System.Web.Mvc** ad alanı ve temel, desteklenen bir parçası olan **System.Web** ad alanı.   
  
MVC, çoğu geliştiricinin bilginiz standart tasarım modeli olur. Web uygulamalarının bazı türleri MVC çerçevesinden faydalanır. Diğer Web Forms ve geri göndermelere dayanan geleneksel ASP.NET uygulama örüntüsünü kullanmaya devam eder. Web uygulamalarının diğer türleri iki yaklaşımı birleştirir; Her iki yaklaşım da diğerini dışlamaz.   
  
MVC çerçevesi aşağıdaki bileşenleri içerir:


[![Bir parametre değeri bekler bir denetleyici eylemi çağırma](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Şekil 01**: bir parametre değeri bekler bir denetleyici Eylemi Çağırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](asp-net-mvc-overview/_static/image2.png))


- **Modelleri**. Model nesneleri uygulama s veri etki alanının mantığını uygulayan uygulamanın parçalarıdır. Genellikle, model nesneleri alabilir ve model durumu bir veritabanında depolar. Örneğin, bir ürün nesnesi bir veritabanından bilgi, üzerinde çalışır ve güncelleştirilmiş bilgileri geri SQL Server'daki bir Ürünler tablosuna yazılamadı.

Küçük uygulamalarda, model ayrım yerine fiziksel bir kavramsal görülür. Uygulama yalnızca bir veri kümesi okur ve görünüme gönderirse, örneğin, uygulamanın fiziksel model katmanı ve ilişkilendirilmiş sınıfları yok. Bu durumda, bir model nesnesi rolünü veri kümesini alır.

- **Görünümleri**. Görünümler uygulama s kullanıcı arabirimi (UI) görüntüleyen bileşenlerdir. Genellikle, bu UI model verilerinden oluşturulur. Örnek metin kutuları, açılan listeleri ve onay kutularını ürünleri nesne geçerli durumuna göre görüntüler Ürünler tablosunun düzenleme görünümü olacaktır.

- **Denetleyicileri**. Denetleyicileri kullanıcı etkileşimini işleyen, modelle çalışan ve sonuçta kullanıcı arabirimini görüntüler oluşturmak için bir görünüm seçin bileşenleridir. Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir. Örneğin, denetleyici sorgu dizesi değerlerini işler ve bu değerleri sırayla değerleri kullanarak veritabanını sorgular modeli geçirir.

MVC örüntüsü, öğeler arasında gevşek bağlantı sağlarken uygulama (giriş mantığı, iş mantığı ve UI mantığı) farklı yönlerini ayıran uygulamalar oluşturmanıza yardımcı olur. Desen burada her bir mantık türünün uygulamanın yer alması gerektiğini belirtir. UI mantığı görünümde yer alır. Giriş mantığı denetleyicide yer alır. İş mantığı modelde yer alır. Bu ayrım, bir uygulama oluşturduğunuzda bir seferde bir yönüne odaklanmanızı sağladığı için karışıklığı yönetmenize yardımcı olur. Örneğin, iş mantığına bağlı kalmaksızın görünüme odaklanabilirsiniz.   
  
Karışıklığın yönetilmesine ek olarak, MVC örüntüsü, uygulamalarının sınanmasından Web Forms tabanlı ASP.NET Web uygulamasını test etmek için daha kolaylaştırır. Örneğin, Web Forms tabanlı ASP.NET Web uygulamasında, hem çıkışı görüntülemek hem de kullanıcı girişine yanıt vermek için tek bir sınıf kullanılır. Belirli bir sayfayı test etmek için sayfa sınıfı, tüm alt denetimleri ve uygulamadaki ek bağımlı sınıfları başlatmanız gerektiği için Web Forms tabanlı ASP.NET uygulamaları için otomatikleştirilmiş testleri yazma karmaşık olabilir. Sayfayı çalıştırmak için çok sayıda sınıf örneği için özel olarak uygulamanın tek tek parçaları üzerinde odaklanan testleri yazmak zor olabilir. Web Forms tabanlı ASP.NET uygulamaları için testleri, bu nedenle bir MVC uygulaması testlerinde daha uygulamak daha zor olabilir. Ayrıca, bir Web Forms tabanlı ASP.NET uygulamasındaki testler bir Web sunucusu gerekir. MVC çerçevesi bileşenleri ayrıştırır ve yalıtım framework geri kalanından bileşenleri tek tek test mümkün kılan arabirimlerin kullanımına ağırlık, yapar.   
  
MVC uygulamasının ilgili üç ana bileşeni arasındaki bu sıkı bağ aynı zamanda paralel gelişimi de kolaylaştırır. Örneğin, bir geliştirici görünümde çalışabilir, ikinci bir geliştirici denetleyici mantığında çalışabilir ve üçüncü Geliştirici üzerinde modeldeki iş mantığına odaklanabilir.

## <a name="deciding-when-to-create-an-mvc-application"></a>Bir MVC uygulaması oluşturmak karar verme

Dikkatlice gerekir mi ASP.NET MVC çerçevesi veya ASP.NET Web Forms modeli kullanarak bir Web uygulaması uygulama. MVC çerçevesi, Web Forms modelini değiştirmez; Web uygulamaları için ikisinden birini kullanabilirsiniz. (Mevcut Web Forms tabanlı uygulamalarınız varsa, bu uygulamalar her zamanki gibi çalışmaya devam edin.)   
  
Belirli bir Web sitesi için MVC çerçevesi veya Web Forms modeli kullanmaya karar vermeden önce her iki yaklaşımın avantajları tartmanız.

### <a name="advantages-of-an-mvc-based-web-application"></a>MVC tabanlı Web uygulamasının avantajları

ASP.NET MVC çerçevesi aşağıdaki avantajları sağlar:

- Bu, uygulama modeli, görünüme ve denetleyiciye bölerek karmaşıklık yönetmenizi kolaylaştırır.
- Görünüm durumu veya sunucu tabanlı formlar kullanmaz. Bu MVC çerçevesi bir uygulamanın davranışı üzerinde tam denetime isteyen geliştiriciler için ideal hale getirir.
- Tek bir denetleyici üzerinden Web uygulama isteklerini işleyen bir Front Controller örüntüsü kullanır. Bu zengin yönlendirme altyapısını destekleyen bir uygulama tasarlamanızı sağlar. Daha fazla bilgi için bkz: [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") MSDN Web sitesinde.
- Teste dayalı geliştirme (TDD) için daha iyi destek sağlar.
- Geliştiriciler ve uygulama davranışı üzerinde denetime yüksek derecede gereksinim duyan Web tasarımcıları büyük ekipleri tarafından desteklenen Web uygulamaları için çalışır.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Web Forms tabanlı Web uygulamasının avantajları

Web Forms tabanlı çerçeve aşağıdaki avantajları sağlar:

- İş Web uygulaması geliştirmeleri için faydalı HTTP üzerinden durumu koruyan olay modelini destekler. Web Forms tabanlı uygulama onlarca, yüzlerce sunucu denetiminde içinde desteklenen olayların sağlar.
- Belirli sayfalara işlevsellik ekleyen Page Controller örüntüsünü kullanır. Daha fazla bilgi için bkz: [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") MSDN Web sitesinde.
- Görünüm durumu veya durum bilgilerinin kolaylaştırabilir sunucu tabanlı formlar kullanır.
- Web geliştiricileri ve hızlı uygulama geliştirme için kullanılabilen bileşeni çok sayıda yararlanmak isteyen tasarımcıları küçük ekipleri için iyi çalışır.
- Genel olarak, uygulama geliştirme için en az karmaşık olduğundan bileşenleri ( **sayfa** sınıfı, denetimleri vb.) sıkı olarak tümleştirildiği ve MVC modeline göre daha az kod genellikle gerektirir.

## <a name="features-of-the-aspnet-mvc-framework"></a>ASP.NET MVC çerçevesinin özellikleri

ASP.NET MVC çerçevesi aşağıdaki özellikleri sağlar:

- Uygulama görevleri (giriş mantığı, iş mantığı ve UI mantığı), sınanabilirlik ve teste dayalı geliştirme (TDD) varsayılan olarak ayrılması. MVC çerçevesindeki tüm esas sözleşmeler arabirim tabanlı ve uygulamadaki asıl nesnelerin davranışını taklit benzetimli nesneler olan sahte nesneler kullanılarak test edilebilir. Birim uygulama Birim hızlı ve esnek olmasını sağlayan bir ASP.NET işlemindeki denetleyicileri çalıştırmak zorunda kalmadan, test. .NET Framework ile uyumlu olan birim testi çerçevesini kullanabilirsiniz.
- Genişletilebilir ve eklenebilir bir çerçeve. ASP.NET MVC çerçevesi bileşenleri, bunlar kolayca değiştirilebilmesi veya özelleştirilebilmesi tasarlanmıştır. Kendi görünüm altyapınızı, URL yönlendirme ilkenizi, eylem yöntemi Parametrenizin serileştirilmesini ve diğer bileşenleri ekleyebilirsiniz. ASP.NET MVC çerçevesi aynı zamanda bağımlılık ekleme (dı) ve denetimi tersine çevirme (IOC) kapsayıcı modellerinin kullanımını destekler. DI, nesnenin kendisini oluşturmak için sınıfa dayanması yerine bir sınıf içine nesneleri Ekle olanak sağlar. IOC, bir nesne başka bir nesneyi gerektirirse, birinci nesnenin ikinci nesneden bir yapılandırma dosyası gibi bir dış kaynaktan alınması gerektiğini belirtir. Bu işlem, testi kolaylaştırır.
- Anlaşılabilir ve aranabilir URL'lere sahip uygulamalar oluşturmanıza olanak sağlayan güçlü bir URL eşlemesi bileşeni. URL'leri dosya adı uzantıları dahil etmek zorunda değildir ve arama için iyi çalışan URL adlandırma modelleri desteklemek üzere tasarlanmış motoru iyileştirmesi (SEO) ve adresleme temsili durum aktarımı (REST).
- Görünüm şablonları olarak mevcut ASP.NET sayfası (.aspx dosyaları), kullanıcı denetimi (.ascx dosyaları) ve ana sayfa (.master dosyaları) biçimlendirme dosyalarında biçimlendirmeyi kullanma desteği. ASP.NET MVC çerçevesi, iç içe geçmiş ana sayfalar gibi mevcut ASP.NET özellikleri kullanmak, satır içi ifadeler (&lt;% = %&gt;), bildirim temelli sunucu denetimleri, şablonlar, veri bağlama, yerelleştirme vb.
- Mevcut ASP.NET özellikleri için destek. ASP.NET MVC, form kimlik doğrulaması ve Windows kimlik doğrulaması, URL yetkilendirmesi, üyelik ve roller, çıkış ve verileri önbelleğe alma, oturum ve profil durumu yönetimi, sistem durumu izleme, yapılandırma sistemi ve sağlayıcı gibi özellikleri kullanmanızı sağlar Mimari.
