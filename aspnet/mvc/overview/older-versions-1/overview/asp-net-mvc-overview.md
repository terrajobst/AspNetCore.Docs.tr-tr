---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET MVC'ye genel bakış | Microsoft Docs
author: microsoft
description: ASP.NET MVC uygulaması ile ASP.NET Web formları uygulamalarını arasındaki farklar hakkında bilgi edinin. Bir ASP.NET MVC uygulamasını oluşturmak ne zaman karar öğrenin.
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 6f40bd19e19f05b91fbee59aefb2914e623d6f25
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802337"
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC'ye genel bakış
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC uygulaması ile ASP.NET Web formları uygulamalarını arasındaki farklar hakkında bilgi edinin. Bir ASP.NET MVC uygulamasını oluşturmak ne zaman karar öğrenin.


Model-View-Controller (MVC) tasarım örüntüsü uygulamanın üç ana bileşene ayırır: model, Görünüm ve denetleyici. ASP.NET MVC çerçevesi, MVC tabanlı Web uygulamaları oluşturmak için ASP.NET Web Forms örüntüsüne bir alternatif sunar. ASP.NET MVC çerçevesi bir basit, yüksek düzeyde sınanabilir bir sunu çerçevesidir, (Web Forms tabanlı uygulamaları olduğu gibi), ana sayfalar ve üyelik tabanlı kimlik doğrulaması gibi mevcut ASP.NET özellikleri ile tümleşiktir. MVC çerçevesi tanımlanan **System.Web.Mvc** ad alanı ve temel, desteklenen bir parçası olan **System.Web** ad alanı.   
  
MVC birçok geliştiricinin aşina olan bir standart bir tasarım örüntüsüdür. Web uygulamalarının bazı türleri MVC çerçevesinden faydalanır. Diğer Web Forms ve geri göndermelere dayanan geleneksel ASP.NET uygulama örüntüsünü kullanmaya devam eder. Web uygulamalarının diğer türleri iki yaklaşımı birleştirir; Her iki yaklaşım da diğerini dışlamaz.   
  
MVC çerçevesi aşağıdaki bileşenleri içerir:


[![Bir parametre değerinin bir denetleyici eylemi çağırma](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Şekil 01**: bir parametre değerinin bir denetleyici Eylemi Çağırma ([tam boyutlu görüntüyü görmek için tıklatın](asp-net-mvc-overview/_static/image2.png))


- **Modelleri**. Model nesneleri uygulama s veri etki alanının mantığını uygulayan uygulamanın bölümlerdir. Genellikle, model nesneleri alabilir ve model durumu bir veritabanında depolar. Örneğin, bir ürün nesnesinin bir veritabanından bilgi üzerinde çalışır ve güncelleştirilmiş bilgileri geri SQL Server'daki bir Ürünler tablosuna yazılamadı.

Küçük uygulamalarda, model, genellikle ayrım yerine fiziksel bir kavramsal olur. Uygulama, yalnızca bir veri kümesini okur ve görünüme gönderirse, örneğin, uygulamanın fiziksel model katmanı ve ilişkilendirilmiş sınıfları yok. Bu durumda, üzerinde bir model nesnesi rolünü veri kümesi alır.

- **Görünümleri**. Görünümler uygulama s kullanıcı arabirimini (UI) görüntüleyen bileşenlerdir. Genellikle, bu UI model verilerinden oluşturulur. Örnek metin kutuları, açılan listeler ve bir ürün nesnesinin mevcut durumuna göre onay kutularını görüntüleyen bir Ürünler tablosunun düzenleme görünümü olacaktır.

- **Denetleyicileri**. Denetleyicileri, kullanıcı etkileşimi işlemek, modelle çalışan ve sonuçta UI görüntüleyen işlemek için bir görünüm seçin bileşenlerdir. Bir MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; Denetleyici, işler ve kullanıcı girişini ve etkileşimini yanıt verir. Örneğin, denetleyici sorgu dizesi değerlerini işler ve bu değerleri sırayla değerlerini kullanarak veritabanını sorgular modeli geçirir.

MVC örüntüsü, öğeler arasında gevşek bir bağlantı sağlayarak uygulama (giriş mantığı, iş mantığı ve UI mantığı) farklı yönlerini birbirlerinden ayıran uygulamalar oluşturmanıza yardımcı olur. Her bir mantık türünün uygulamanın nerede bulunmalıdır desenini belirtir. UI mantığı görünümde yer alır. Giriş mantığı denetleyicide yer alır. İş mantığı modelde yer alır. Bu ayrım uygulamasının aynı anda tek bir yönüne odaklanmanızı sağlar çünkü bir uygulama oluşturduğunuzda karmaşıklığını yönetmenize yardımcı olur. Örneğin, iş mantığına bağlı kalmaksızın görünüme odaklanabilirsiniz.   
  
Karmaşıklığı yönetmenin yanı sıra, MVC örüntüsü, Web Forms tabanlı ASP.NET Web uygulamasını test etmek için uygulamalarının sınanmasından kolaylaştırır. Örneğin, bir Web Forms tabanlı ASP.NET Web uygulamasında, hem çıkışı görüntülemek hem de kullanıcı girişine yanıt vermek için tek bir sınıf kullanılır. Tek bir sayfayı test etmek üzere sayfa sınıfı, tüm alt denetimleri ve uygulamadaki ek bağımlı sınıfları başlatmanız gerektiği için Web Forms tabanlı ASP.NET uygulamaları için otomatik testler yazmak karmaşık olabilir. Sayfayı çalıştırmak için çok sayıda sınıf örneği oluşturulur çünkü özel olarak tek tek uygulama kısımlarına odaklanmak testleri yazmak zor olabilir. Bu nedenle testleri Web Forms tabanlı ASP.NET uygulamaları için MVC uygulamasındaki testler uygulanmasından çok daha zor olabilir. Ayrıca, bir Web Forms tabanlı ASP.NET uygulamasındaki testler bir Web sunucusu gerekir. MVC çerçevesi bileşenleri ayrıştırır ve tek tek bileşenlerin çerçevenin geri kalanındaki yalıtım test mümkün ağır arabirimleri kullanır.   
  
Bir MVC uygulamasının ilgili üç ana bileşeni arasındaki bu sıkı bağ aynı zamanda paralel gelişimi de kolaylaştırır. Örneğin, bir geliştirici görünümde çalışabilir, ikinci bir geliştirici denetleyici mantığında çalışabilir ve üçüncü Geliştirici modelinde iş mantığı üzerinde odaklanabilir.

## <a name="deciding-when-to-create-an-mvc-application"></a>Ne zaman bir MVC uygulaması oluşturma belirleme

Dikkatli bir şekilde dikkate almanız gereken ASP.NET MVC çerçevesi veya ASP.NET Web Forms modeli kullanarak bir Web uygulaması uygulanmayacağını. MVC çerçevesi, Web Forms modelini değiştirmez; Web uygulamaları için ikisinden birini kullanabilirsiniz. (Mevcut Web Forms tabanlı uygulamalarınız varsa, bunlar tam olarak her zaman olduğu gibi çalışmaya devam edin.)   
  
Belirli bir Web sitesi için MVC çerçevesi veya Web Forms modeli kullanmaya karar vermeden önce her iki yaklaşımın avantajlarını masaya yatırın.

### <a name="advantages-of-an-mvc-based-web-application"></a>Bir MVC tabanlı Web uygulamasının avantajları

ASP.NET MVC çerçevesi aşağıdaki avantajları sunar:

- Bu karmaşıklığı, model, Görünüm ve denetleyici içinde uygulama bölerek Karışıklığın yönetilmesini kolaylaştırır.
- Görünüm durumu veya sunucu tabanlı formlar kullanmaz. Bu MVC çerçevesi bir uygulamanın davranışı üzerinde tam denetim isteyen geliştiriciler için ideal hale getirir.
- Tek bir denetleyici üzerinden Web uygulama isteklerini işleyen bir Front Controller örüntüsü kullanır. Bu, zengin yönlendirme altyapısını destekleyen bir uygulama tasarlamanızı sağlar. Daha fazla bilgi için [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") MSDN Web sitesinde.
- Teste dayalı geliştirme (TDD) için daha iyi destek sağlar.
- Geliştiriciler ve uygulama davranışı üzerinde denetim yüksek derecede ihtiyaç duyan Web tasarımcıları büyük takımlar tarafından desteklenen Web uygulamaları için çalışır.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Web Forms tabanlı Web uygulamasının avantajları

Web Forms tabanlı çerçeve aşağıdaki avantajları sunar:

- Bu satır iş kolu Web uygulaması geliştirmeleri için faydalı HTTP üzerinden durumu koruyan olay modelini destekler. Web Forms tabanlı uygulama onlarca, yüzlerce sunucu denetiminde içinde desteklenen olayları sağlar.
- Belirli sayfalara işlevsellik ekleyen Page Controller örüntüsünü kullanır. Daha fazla bilgi için [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") MSDN Web sitesinde.
- Görünüm durumu veya durum bilgilerinin kolaylaştırabilir sunucu tabanlı formlarda kullanır.
- Web geliştiricileri ve tasarımcıları bileşenleri hızlı uygulama geliştirme için kullanılabilir çok sayıda yararlanmak isteyen küçük takımlar için iyi çalışır.
- Genel olarak, uygulama geliştirme için daha az karmaşık olduğundan bileşenleri ( **sayfa** sınıfı, denetimleri vb.) sıkı olarak tümleştirildiği ve MVC modeline göre daha az kod genellikle gerektirir.

## <a name="features-of-the-aspnet-mvc-framework"></a>ASP.NET MVC çerçevesinin özellikleri

ASP.NET MVC çerçevesi aşağıdaki özellikleri sağlar:

- Uygulama görevleri (giriş mantığı, iş mantığı ve UI mantığı), sınanabilirlik ve teste dayalı geliştirme (TDD) varsayılan olarak ayrılması. MVC çerçevesindeki tüm esas sözleşmeler arabirim tabanlı ve uygulama asıl nesnelerin davranışını taklit eden benzetimli nesneler olan sahte nesneler kullanılarak test edilebilir. Birimi uygulama Birim hızlı ve esnek bir ASP.NET işlemindeki denetleyicileri çalıştırmak zorunda kalmadan test. .NET Framework ile uyumlu olan birim testi çerçevesini kullanabilirsiniz.
- Genişletilebilir ve eklenebilir bir çerçeve. ASP.NET MVC çerçevesi bileşenleri, bunlar kolayca veya özelleştirilebilmesi tasarlanmıştır. Kendi görünüm altyapınızı, URL yönlendirme ilkenizi, eylem-yöntem serileştirilmesini ve diğer bileşenleri takabilirsiniz. ASP.NET MVC çerçevesi, bağımlılık ekleme (dı) ve denetimi tersine çevirme (IOC) kapsayıcı modellerinin kullanımını da destekler. DI, nesneler nesneyi oluşturmak için sınıfa kalmak yerine, bir sınıf eklemeye sağlar. IOC, bir nesne başka bir nesneyi gerektirirse, birinci nesnenin ikinci nesneden bir yapılandırma dosyası gibi bir dış kaynaktan alınması gerektiğini belirtir. Bu, testi kolaylaştırır.
- Anlaşılabilir ve aranabilir URL'lere sahip uygulamalar oluşturmanızı sağlayan güçlü bir URL eşlemesi bileşeni. URL'leri dosya adı uzantıları dahil etmek zorunda değildir ve arama çalışan URL adlandırma modelleri destekleyecek şekilde tasarlanmıştır motoru iyileştirmesi (SEO) ve adresleme temsili durum aktarımı (REST).
- Görünüm şablonları olarak mevcut ASP.NET sayfası (.aspx dosyaları), kullanıcı denetimi (.ascx dosyaları) ve ana sayfa (.master dosyaları) biçimlendirme dosyalarında biçimlendirmeyi kullanma desteği. ASP.NET MVC çerçevesi, iç içe geçmiş ana sayfalar gibi mevcut ASP.NET özellikleri kullanmak, satır içi ifadeler (&lt;% = %&gt;), bildirim temelli sunucu denetimleri, şablonlar, veri bağlama, yerelleştirme ve benzeri.
- Mevcut ASP.NET özellikleri için destek. ASP.NET MVC, form kimlik doğrulaması ve Windows kimlik doğrulaması, URL yetkilendirmesi, üyelik ve roller, çıkış ve verileri önbelleğe alma, oturum ve profil durumu yönetimi, sistem durumu izleme, yapılandırma sistemi ve sağlayıcı gibi özellikleri kullanmanıza olanak tanır. Mimari.
