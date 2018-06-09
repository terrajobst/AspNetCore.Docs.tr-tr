---
uid: single-page-application/overview/templates/breezeangular-template
title: Kolay/Angular şablonu | Microsoft Docs
author: madskristensen
description: Kolay/Angular tek sayfa uygulaması şablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566313"
---
<a name="breezeangular-template"></a>Kolay/Angular şablonu
====================
tarafından [Kristensen Mads](https://github.com/madskristensen)

> Kolay/Angular MVC şablonu tarafından Atla zil yazıldı
> 
> [Kolay/Açısal MVC şablonu indirme](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) tek sayfa uygulamaları (SPAs) oluşturmak için bir açık kaynak Kitaplığı'ndan Google değil. Veri bağlama, bağımlılık ekleme ve ekran Yönetimi sunar. İle birleştirerek [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), başka bir açık kaynak kitaplığı veri modellemesi ve veri yönetimi ve sizin için harika bir HTML/JavaScript istemci uygulama için temel malzemeleri sahip.

Bir değişim açıktır kolay/Angular SPA şablonu [Çakıştırmaları SPA şablonu](../introduction/knockoutjs-template.md) ASP.NET ve Web Araçları 2012.2 güncelleştirmesi dahil. Visual Studio ekranınız varsa, bir örnek SPA hazır ve çalışır 60 saniyeden daha kısa bir süre içinde sahip olacaksınız.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Outwardly, uygulama için çok benzer Çakıştırmaları SPA şablon arar. Ancak bu başlık altında oldukça farklı. Çakıştırmaları şablon Boşaltılan veri bağlama ve veri erişimi için ham AJAX kullanır. Kolay/Angular şablonu Angular veri erişimi için veri bağlama ve kolay için kullanır. Bu kitaplıkları sayfa gezintisi ve geçmişi gibi ek özellikleri sağlar.

Uygulamanın hakkında sayfa şöyledir:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Bu sayfa, geçerli kullanıcının oturumu sırasında çalışan günlük olaylarının görüntüler dahil olmak üzere:

- Disk belleği. #2 ve #7 Yapılacaklar denetleyicisi oluşturmanın unutmayın.
- Uzak sorgular (#3) ve yerel önbellek sorgular (#7).
- Yeni kaydetme (5, #6) ve (#4) varlıklar değiştirilebilir.
- Kullanıcı değişiklikleri kaydetmeden önce hataları düzeltebilir şekilde (#9), istemcide veritabanına doğrulanmış değiştirir.

Bu şablonda keşfetmek için daha fazla yoktur dahil olmak üzere:

- HTML görünümü şablonları dinamik yüklenmesi.
- Özel veri bağlama aracılığıyla Angular "yönergeleri."
- Modüler ve bağımlılık ekleme.
- Sorgu filtreleri, sıralar, disk belleği, tahminleri ve ilgili varlıkların ekleme.
- Veri birden çok ekranları arasında paylaştırma.
- Birden fazla değişiklik tek bir işlem olarak kaydediliyor.
- Doğrulama kuralları sunucudan JavaScript istemciye otomatik olarak yayılır.

Haydi başlayalım.

## <a name="create-a-breezeangular-template-project"></a>Kolay/Açısal Şablonu proje oluşturma

İndirin ve şablonu yukarıdaki karşıdan yükleme düğmesini tıklatarak yükleyin. Şablonu bir Visual Studio Uzantısı (VSIX) dosyası olarak paketlenir. Visual Studio yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Proje adı ve'ı tıklatın **Tamam**.

İçinde **yeni proje** seçin **kolay Açısal SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Derleme ve hata ayıklama olmadan uygulamayı çalıştırmak için CTRL-F5 tuşuna basın veya hata ayıklama ile çalıştırmak için F5 tuşuna basın.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Bu etkinlikler günlüğünü görmek için sağ üst "About" bağlantısını tıklatın.

Doğrulama mantığını kolay tarafından gerçekleştirilen istemci-tarafı ' dir. Sunucu modeli sınıfları doğrulama öznitelikleri istemciye yayılan ve istemci sunucusuyla bağlantı kurar önce otomatik olarak yürütülür.

Ağ trafiği gözden geçirin. Kolay bir hata algılandığında sunucuya hiçbir çağrılar olduğunu dikkat edin. Her geçerli değişikliği bir POST isteğinin sonuçlandı "/ Todo/api/SaveChanges". Kolay değişiklikleri sunmaktadır ve Web API denetleyicinin birlikte tek bir istek gönderir `SaveChanges` yöntemi. PUT, POST ve silme istekleri her öğe için ayrı ayrı yapar KockoutJS SPA şablonundan farklı olmasıdır.

Ayrıca, sayfaları hakkında TodoList arasında geçiş yaptığınızda hiçbir ağ trafiğini olduğuna dikkat edin. Sorgu yerel kolay önbellek kısıtlı olmasıdır.

## <a name="peek-inside"></a>İçinde Gözat

Bu uygulama, istemci tarafı ve sunucu tarafındaki vardır. Üçüncü taraf JavaScript kitaplıklarını ("Betikleri" klasöründe) artı küçük bir HTML ve uygulama JavaScript modülleri ("uygulama" klasöründe) birleşimini, istemci-tarafı yığını oluşur.

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Kullanıcı Arabirimi mimarisi HTML pencere öğeleri görünümlerinin denetleyicileri destekleyen sunu koddan ayırır. Her, diğer intimate bilgisi olmadan işini yapabilmesi için Açısal veri bağlama sistem görünümleri ve denetleyicilerini düzenler.

Denetleyici edinmeli ve model varlıklar kaydetmek için veri bağlamı sorar. Veri bağlamı için JSON sorgu sonuçları kendini izleyen model nesneleri oluşturur kolay işlerin çoğunu atar.

Sunucu tarafı yığını bazı geliştirici kodu ve üç ilkesi .NET kitaplıklarına oluşur: Web API, Entity Framework ve Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Temel mimari KockoutJS SPA şablonu ile aynıdır. Ancak, uygulama çok daha kolaydır: DTOs silindi ve Entity Framework ayrıntıları çoğunu Breeze.NET için temsilci seçilmiş.

## <a name="next-steps"></a>Sonraki Adımlar

Tarafından destekli kodu, keşfetme önerdiğimiz [kapsamlı tartışma](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) hem istemci hem de sunucu yığınları kolay Web sitesi.

Kolay istemci-tarafı sorgusu yürütmeyi deneyin; Bazı filtreler ve sıralar ekleyin. Daha fazla model özellikleri ve uçtan uca SPA geliştirme için daha iyi bir fikir almak için daha fazla varlık ekleyebilirsiniz. Tasarımını olduğunuzda Yapılacaklar özellikleri kesmeden ve bunları kendi ile değiştirin.

Mutluluk kodlama!
