---
uid: single-page-application/overview/templates/breezeangular-template
title: BREEZE/Angular şablonu | Microsoft Docs
author: madskristensen
description: BREEZE/Angular tek sayfalı uygulama şablonu
ms.author: aspnetcontent
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3799c891625b28312b54ed33628748dcc1b74925
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811756"
---
<a name="breezeangular-template"></a>BREEZE/Angular şablonu
====================
tarafından [Mads Kristensen](https://github.com/madskristensen)

> Breeze/Angular MVC şablonu Atla zil tarafından yazılmıştır.
> 
> [Breeze/Angular MVC şablonu indirin](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) tek sayfa uygulamaları (Spa'lar) oluşturmaya yönelik bir açık kaynak kitaplığı google'dan. Veri bağlama, bağımlılık ekleme ve ekranı Yönetimi sunar. İle birlikte [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), başka bir açık kaynak kitaplığı veri modelleme ve veri yönetimi ve sizin için harika bir HTML/JavaScript istemci uygulaması için gerekli malzemeleri sahip.

SPA Breeze/Angular şablonu bir değişim açıktır [SPA KnockoutJS şablonu](../introduction/knockoutjs-template.md) ASP.NET ve Web Araçları 2012.2 güncelleştirme dahil. Visual Studio kendinizi yapıyorsanız, örnek bir SPA çalışmaya 60 saniyeden kısa bir süre içinde sahip olursunuz.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Edilemezler, uygulama KnockoutJS SPA şablona çok benzer görünür. Ancak, bileşenler oldukça farklıdır. KnockoutJS şablonu Knockout veri bağlamayı ve veri erişimi için ham AJAX için kullanır. Breeze/Angular şablonu Angular veri erişimi için veri bağlama ve Breeze için kullanır. Bu kitaplıkları sayfa gezintisi ve geçmiş gibi ek özellikler sağlar.

Uygulamanın hakkında sayfası aşağıda verilmiştir:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Bu sayfa, geçerli kullanıcının oturumu sırasında çalışan bir günlüğe görüntüler dahil olmak üzere:

- Disk belleği. #2 ve #7 Todo denetleyicisi oluşturmayı unutmayın.
- Uzak sorgular (3) ve yerel önbellek sorgular (#7).
- Yeni kaydediliyor (5, #6) ve (4) varlıklar değiştirilebilir.
- Değişiklikler, kullanıcı değişiklikleri yapmadan önce hataları düzeltmek için veritabanına (#9), istemci üzerinde doğrulanır.

Bu şablonda keşfetmek için devamı dahil olmak üzere:

- Dinamik HTML görünümü şablonları yükleme.
- Özel veri bağlama aracılığıyla Angular "yönergeleri."
- Modüler ve bağımlılık ekleme.
- Sorgu filtreleri, sıralama, sayfalama, tahminler ve ilgili varlıkları dahil edilmesi.
- Birden çok ekranlar arasında veri paylaşımı.
- Birden fazla değişiklik tek bir işlem olarak kaydediliyor.
- Doğrulama kuralları, sunucudan JavaScript istemciye otomatik olarak yayılır.

Haydi başlayalım.

## <a name="create-a-breezeangular-template-project"></a>Breeze/Angular Şablonu proje oluşturma

İndirin ve yukarıdaki indir düğmesine tıklayarak şablonu yükleyin. Şablon, Visual Studio Uzantısı (VSIX) dosyası olarak paketlenir. Visual Studio'yu yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Projeyi adlandırın ve tıklayın **Tamam**.

İçinde **yeni proje** seçin **Breeze Angular SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Derleme ve hata ayıklama olmadan uygulamayı çalıştırmak için CTRL-F5 tuşuna basın veya hata ayıklamayla çalıştırmak için F5 tuşuna basın.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Uygulamayı ilk kez çalıştırdığında, oturum açma ekranını görüntüler. "Kaydolma" bağlantısına tıklayın ve yeni bir sayfa görünüme, bir kullanıcı adı ve parola girebileceğiniz glides. (Oturum açma ve kayıt sayfaları ASP.NET MVC kullanılarak oluşturulur.) Kayıt formu gönderdiğinde, sunucu, hesabınız için iki öğeli bir Yapılacaklar listesi oluşturur. Ardından, bunları size sarı bir not üzerinde sunar.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

' In kara SPA sunulmuştur. Her şeyi görmek ve istemci Knockout ve Breeze yardımıyla üzerinde yönetilen ve işlenen açıklamada düzenleme deneyimi. Uygulama, bir kullanıcı olarak araştırın... Ancak bir geliştiricinin göz. Ağ trafiğini yakalamak için tarayıcınızda geliştirici araçlarını kullanın. (Internet Explorer'da: F12 tuşuna basın, select **ağ** sekmesine ve tıklayın **Yakalamayı Başlat**.) Şimdi aşağıdakileri deneyin:

- Yeni bir Todo öğesini ekleyin.
- Etiket tıklayın ve Todo öğesi başlığını Düzenle
- Öğesi tamamlandı olarak işaretlemek için bir onay kutusunu işaretleyin. Başlığın artık düzenlenemez, bu nedenle, metin kutusu devre dikkat edin.
- Etiketin sağında 'x' tıklayın. Öğe kaybolur ve veritabanından silinir.
- Başka bir öğe seçin ve başlığını temizleyin. Başlık gereklidir bir doğrulama hatası alırsınız. Kısa bir duraklamadan sonra önceki başlığı geri yüklenir.
- Gerçekten uzun bir başlık yazın. Başlık çok uzun farklı bir doğrulama hatası alırsınız.
- "Yapılacaklar Listesi Ekle" düğmesine tıklayın. Yeni bir liste, önceki listede solunda görünür.
- TodoList başlık, kendi gerekli tetikleme ve uzunluğu doğrulamaları ile yürütün.
- Hata iletisi temizlemek için başlığı metin kutusuna tıklayın.
- Daire TodoList ve kendi açıklamada silmek için sağ üst köşedeki "x"'a tıklayın.
- Bu etkinliklerin bir günlüğünü görmek için sağ üst köşedeki "Hakkında" bağlantısına tıklayın.

Doğrulama mantığını Breeze tarafından gerçekleştirilen istemci-tarafı ' dir. Doğrulama öznitelikleri sunucusu model sınıfları istemciye yayılır ve sunucunun istemci irtibata geçmeden önce otomatik olarak yürütülür.

Ağ trafiği gözden geçirin. Meltem bir hata algılandığında sunucuya çağrı olduğunu dikkat edin. Geçerli her değişiklik, bir POST isteğinde "/ Todo/API/SaveChanges" sonuçlandı. Meltem değişiklikleri oluşturur ve Web API denetleyicinin birlikte tek bir istek gönderir `SaveChanges` yöntemi. GÖNDERİN ve her öğe için olan istekleri ayrı ayrı silmek yerine getiren KockoutJS SPA şablondan farklı olmasıdır.

Ayrıca, sayfaları hakkında TodoList arasında geçiş yaptığınızda hiçbir ağ trafiği olduğuna dikkat edin. Sorgu yerel Breeze önbelleğe kısıtlı olmasıdır.

## <a name="peek-inside"></a>İçinde Özet

Bu uygulama, istemci tarafı ve sunucu tarafı sahiptir. Üçüncü taraf JavaScript kitaplıkları ("Komut" klasöründe) artı küçük bir HTML ve uygulama JavaScript modüllerinde ("uygulama" klasöründe) birleşimi, istemci tarafı yığın oluşur.

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

UI mimarisi HTML pencere öğeleri görünüm denetleyicileri destekleyici sunu kodundan ayırır. Böylece her diğer tutun bilgisi olmadan işini yapabilirsiniz Angular veri bağlama sistem görünümleri ve denetleyicileri düzenler.

Denetleyici elde ve modeli varlıklarından kaydetmek için veri bağlamı ister. Veri bağlamı için JSON sorgu sonuçlarından Self izleme model nesneleri oluşturan Meltem, işin çoğunu atar.

Sunucu tarafı yığın Geliştirici kod ve üç ilkesi .NET kitaplıkları oluşur: Web API'si, Entity Framework ve Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Temel mimari KockoutJS SPA şablonu ile aynıdır. Ancak, uygulama çok daha kolaydır: Dto'lar silindi ve Entity Framework ayrıntılarını çoğunu Breeze.NET için yönetici temsilcisi.

## <a name="next-steps"></a>Sonraki Adımlar

Tarafından destekli kod keşfedin öneririz [kapsamlı tartışma](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) hem istemci hem de sunucu yığınları Breeze Web sitesinde.

Meltem istemci-tarafı sorgu yürütmeyi deneyebilirsiniz; Bazı filtreleri ve sıralamayı ekleyin. Daha fazla model özellikleri ve uçtan uca SPA geliştirme için daha iyi bir genel görünüm almak için daha fazla varlık ekleyebilirsiniz. Tasarımını olduğunuzda Todo özellikleri ayırma ve bunları kendi değerlerinizle değiştirin.

İyi Kodlamalar!
