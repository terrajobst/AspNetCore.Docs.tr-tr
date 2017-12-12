---
uid: single-page-application/overview/templates/breezeknockout-template
title: "Kolay/Boşaltılan şablon | Microsoft Docs"
author: madskristensen
description: "Kolay/Boşaltılan tek sayfa uygulaması şablonu"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>Kolay/Boşaltılan şablonu
====================
tarafından [Kristensen Mads](https://github.com/madskristensen)

> Kolay/Boşaltılan MVC şablonu tarafından Atla zil yazıldı
> 
> [Kolay/Boşaltılan MVC şablonu indirme](https://go.microsoft.com/fwlink/?LinkId=282649)


"Tek sayfa uygulamasının" duymadığınız (SPA) ve ne olduğunu hiç merak ettiniz. Bu konuda okuyabilir, ancak, yerine onu yaşamak için. Ancak bir örnek yükleme süresi olanların? Visual Studio ekranınız varsa, örneği SPA gerekir ve içinde değerinden 60 çalıştıran ile ASP.NET MVC 4 "Kolay/Boşaltılan tek sayfa uygulaması" şablonu saniye.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Kolay/Boşaltılan SPA şablonu nedir?

Çoğu proje şablonları bir uygulama çatıyı oluşturur. Kodunuzu ekleyerek bu kemikler üzerinde flesh yerleştirin ve sonunda çalışan bir uygulama sunmak. Kolay/Boşaltılan SPA şablon farklıdır. Araştırmak örnek bir uygulama oluşturur. SPA uygulama tasarımı ve bir SPA oluşturmak için kullanılan teknikleri çoğunu gösterir.

Bir değişim açıktır kolay/Boşaltılan şablonu [Çakıştırmaları SPA şablonu](../introduction/knockoutjs-template.md) ASP.NET ve Web Araçları 2012.2 güncelleştirmesi dahil. Aynı kullanıcı deneyimini uygulamayla kolay SPA şablon oluşturur, ancak veri yönetimi için kolay kullanarak farklı bir uygulama bulunur.

Çakıştırmaları SPA şablon, basit bir uygulama için yeterli olduğu ham jQuery AJAX, olan hizmet istekleri hale getirir. Ancak daha karmaşık uygulamalar gittikçe veri yönetim gereksinimleri vardır. Örneğin, çoğu uygulamalar:

- Sorgulamak ve sunucunun genişletilmiş kullanıcı oturumu sırasında yeniden sorgular.
- Sıralama ve disk belleği sorgu filtreleri ekleyin.
- Aynı verileri birden çok ekranları arasında paylaşın.
- Birçok nesnelerdeki değişiklikleri accumulate, sonra bunları tek bir işlem olarak kaydedin.
- Kullanıcı veritabanına değişiklikleri kaydetmeden önce hataları düzeltmek için istemcideki değişikliklerin doğrulayın.

BreezeJS kitaplığı bu, sizin için en önemli uygulama mantığı ve kullanıcı deneyimini geliştirmek için boşaltma işler.

[**Kolay** ](http://www.breezejs.com/?utm_source=ms-spa) JavaScript ve HTML, geçmişte tek başına Masaüstü uygulamaları teslim uygulamaları türlerini zengin veri uygulamaları oluşturmak için bir açık kaynak kitaplığı.

Kolay/Boşaltılan şablonu daha güçlü bir veri yönetimi altyapı ilk bu önemli adım almanıza yardımcı olur. Çakıştırmaları SPA şablona outwardly aynı olan bir örnek Todo uygulaması oluşturur. İç, AJAX veri katmanı ile kolay yerini alır, iki karşılaştırmak için yan yana yaklaşıyor. Elbette, bir kolay uygulama olasılığı neredeyse hiç dokunur. Ancak, kolay nasıl çalıştığını görürsünüz ve nasıl biraz geçiş yapmak için gereklidir.

Haydi başlayalım.

## <a name="create-a-breezeknockout-template-project"></a>Bir kolay/Boşaltılan şablonu projesi oluşturma

İndirin ve şablonu yukarıdaki karşıdan yükleme düğmesini tıklatarak yükleyin. Şablonu bir Visual Studio Uzantısı (VSIX) dosyası olarak paketlenir. Visual Studio yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#**seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Proje adı ve'ı tıklatın **Tamam**.

İçinde **yeni proje** seçin **kolay Boşaltılan SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Derleme ve hata ayıklama olmadan uygulamayı çalıştırmak için CTRL-F5 tuşuna basın veya hata ayıklama ile çalıştırmak için F5 tuşuna basın.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Uygulamayı ilk kez çalıştırdığında, oturum açma ekranı görüntüler. "Kaydolma" bağlantısına tıklayın ve bir kullanıcı adı ve parola girebileceğiniz yeni bir sayfa görünümü içine glides. (Oturum açma ve kayıt sayfaları ASP.NET MVC kullanılarak oluşturulur.) Kayıt formu gönderdiğinde, sunucu, hesabınız için iki öğeli bir Yapılacaklar listesi oluşturur. Ardından, bunları size sarı bir not üzerinde gösterir.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Artık, kara SPA bulunur. Her şeyi, görebilir ve Todos düzenleme işlenmiş ve Knockout ve kolay Yardım ile istemcide yönetilen karşılaşırsınız. Uygulama bir kullanıcı olarak keşfedin... Ancak bir geliştiricinin göz. Ağ trafiğini yakalamak için tarayıcınızda geliştirici araçlarını kullanın. (Internet Explorer'da: F12 tuşuna basın, select **ağ** sekmesine ve tıklayın **Yakalamayı Başlat**.) Şimdi aşağıdakileri deneyin:

- Yeni bir Yapılacaklar öğesi ekleyin.
- Etiket'i tıklatın ve Yapılacaklar öğesi başlık Düzenle
- Öğesi Bitti'yi işaretlemek için onay kutusunu işaretleyin. Başlık artık düzenlenemez şekilde textbox devre dikkat edin.
- Etiket sağındaki 'x' tıklayın. Öğe kaybolur ve veritabanından silinir.
- Başka bir öğe seçin ve başlığını temizleyin. Başlık gerekli bir doğrulama hata iletisi alırsınız. Kısa bir duraklamadan sonra önceki başlık geri yüklendi.
- Gerçekten uzun bir başlık yazın. Başlık çok uzun olduğundan farklı doğrulama hatası alırsınız.
- "Yapılacaklar listesine ekle" düğmesini tıklatın. Yeni bir liste önceki listede solunda görünür.
- TodoList başlık, gerekliyse tetikleme ve uzunluğu doğrulamaları yürütün.
- Hata iletisi temizlemek için başlık metin kutusuna tıklayın.
- Daire içinde TodoList ve kendi todos silmek için sağ üst köşesindeki "x"'i tıklatın.

Doğrulama mantığını kolay tarafından gerçekleştirilen istemci-tarafı ' dir. Sunucu modeli sınıfları doğrulama öznitelikleri istemciye yayılan ve istemci sunucusuyla bağlantı kurar önce otomatik olarak yürütülür.

Ağ trafiği gözden geçirin. Kolay bir hata algılandığında sunucuya hiçbir çağrılar olduğunu dikkat edin. Her geçerli değişikliği bir POST isteğinin sonuçlandı "/ Todo/api/SaveChanges". Kolay değişiklikleri sunmaktadır ve Web API denetleyicinin birlikte tek bir istek gönderir `SaveChanges` yöntemi. PUT, POST ve silme istekleri her öğe için ayrı ayrı yapar KockoutJS SPA şablonundan farklı olmasıdır.

## <a name="peek-inside"></a>İçinde Gözat

Bu uygulama, istemci tarafı ve sunucu tarafındaki vardır. Üçüncü taraf JavaScript kitaplıklarını ("Betikleri" klasöründe) artı küçük bir HTML ve uygulama JavaScript modülleri ("uygulama" klasöründe) birleşimini, istemci-tarafı yığını oluşur.

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Çakıştırmaları SPA şablon araştırılan varsa, bu tanıdık gelecektir. Mavi kutuları odaklanın. Model-View-ViewModel (hangi görünümün HTML pencere öğeleri düzgün bir şekilde görünüm modeli destekleyen sunu koddan ayrılmış MVVM), kullanıcı Arabirimi mimarisidir. Her, diğer intimate bilgisi olmadan işini yapabilmesi için bir veri bağlama sistemine (Bu durumda Boşaltılan) görünümü ve görünüm modeli düzenler.

Model Yapılacaklar verileri saklar. Görünümünde pencere öğeleri için doğrudan bağlanabilir şekilde modeldeki varlıklar Boşaltılan observable özelliklerle kolay tarafından oluşturulur. Görünüm modeli edinmeli ve model varlıklar kaydetmek için veri bağlamı sorar. Veri bağlamı kolay işin çoğunu atar.

Sunucu tarafı yığını bazı geliştirici kodu ve üç ilkesi .NET kitaplıklarına oluşur: Web API, Entity Framework ve Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Temel mimari KockoutJS SPA şablonu ile aynıdır. Ancak, uygulama çok daha kolaydır: DTOs silindi ve Entity Framework ayrıntıları çoğunu Breeze.NET için temsilci seçilmiş.

## <a name="next-steps"></a>Sonraki Adımlar

Tarafından destekli kodu, keşfetme önerdiğimiz [kapsamlı tartışma](http://www.breezejs.com/spa-template?utm_source=ms-spa) hem istemci hem de sunucu yığınları kolay Web sitesi.

Kolay istemci-tarafı sorgusu yürütmeyi deneyin; Bazı filtreler ve sıralar ekleyin. Daha fazla model özellikleri ve uçtan uca SPA geliştirme için daha iyi bir fikir almak için daha fazla varlık ekleyebilirsiniz. Tasarımını olduğunuzda Yapılacaklar özellikleri kesmeden ve bunları kendi ile değiştirin.

Hemen bir sonraki büyük adım için hazır olacak: istemci-tarafı ekranlar ekleme ve bunlar arasında gezinme. Bu SPA şablon ardınızda ve bir daha kapsamlı SPA yığınına gibi açın [John Papa'nın etkin havlu](https://github.com/johnpapa/HotTowel#readme "etkin havlu"), kolay ve çakıştırma Karışıma Durandal ekler.
