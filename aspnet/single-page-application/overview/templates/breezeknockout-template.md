---
uid: single-page-application/overview/templates/breezeknockout-template
title: BREEZE/Knockout şablonu | Microsoft Docs
author: madskristensen
description: BREEZE/Knockout tek sayfalı uygulama şablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 48ee0463fe950c28832523986a2242417411c96a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376870"
---
<a name="breezeknockout-template"></a>BREEZE/Knockout şablonu
====================
tarafından [Mads Kristensen](https://github.com/madskristensen)

> Breeze/Knockout MVC şablonu Atla zil tarafından yazılmıştır.
> 
> [Breeze/Knockout MVC şablonu indirin](https://go.microsoft.com/fwlink/?LinkId=282649)


"Tek sayfalı uygulama" çok sayıda öneri aldık (SPA) ve bunun ne olduğunu merak. Bu konuda okuyabilir, ancak, bunun yerine, kendiniz deneyimleyeceği. Ancak, bir örnek yüklemek için süre kimler? Visual Studio kendinizi örneği SPA gerekir ve ASP.NET MVC 4 "Breeze/Knockout tek sayfalı uygulama" şablonu saniye içinde değerinden 60'ı çalıştıran.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Breeze/Knockout SPA şablonu nedir?

Çoğu proje şablonları, bir uygulama çatısı oluşturur. Kod ekleyerek bu kemikler üzerinde beklemiyoruz yerleştirin ve sonunda çalışan bir uygulama sunun. Breeze/Knockout SPA şablon farklıdır. Bu, üzerinde çalışmanız örnek bir uygulama oluşturur. Bu, bir SPA uygulama tasarımı ve birçok bir SPA oluşturmaya yönelik teknikleri gösterir.

Breeze/Knockout şablonu bir değişim açıktır [SPA KnockoutJS şablonu](../introduction/knockoutjs-template.md) ASP.NET ve Web Araçları 2012.2 güncelleştirme dahil. Breeze SPA şablon aynı kullanıcı deneyimi ile bir uygulama oluşturur, ancak Breeze veri yönetimi için kullanarak, farklı bir uygulama vardır.

SPA KnockoutJS şablonu, basit bir uygulama için yeterli olan ham jQuery, AJAX olan hizmet istekleri yapar. Ancak, daha gelişmiş uygulamalar daha zorlu veri yönetimi gereksinimlerine sahip. Örneğin, çoğu uygulama:

- Sorgulamak ve bir genişletilmiş kullanıcı oturumu sırasında sunucu için yeniden sorgular.
- Sıralama ve disk belleği sorgu filtreleri ekleyin.
- Aynı verileri birden çok ekranlar arasında paylaşın.
- Çok sayıda nesne değişiklikleri accumulate, sonra bunları tek bir işlem olarak kaydedin.
- Kullanıcı veritabanına değişiklikleri yapmadan önce hataları düzeltmek için istemci üzerindeki değişiklikleri doğrulayın.

Bu, sizin için en önemli uygulama mantığını ve kullanıcı deneyimini geliştirmek için serbest bırakma BreezeJS kitaplığı işler.

[**Meltem** ](http://www.breezejs.com/?utm_source=ms-spa) zengin veri uygulamalarında JavaScript ve HTML, tür tarihsel olarak tek başına bir masaüstü uygulaması olarak sunulan uygulamalar oluşturmaya yönelik bir açık kaynak kitaplığı.

Breeze/Knockout şablonu daha sağlam bir veri yönetim altyapısı ilk bu önemli adım yararlanmanıza yardımcı olur. Bu, KnockoutJS SPA şablona edilemezler aynı olan örnek bir Todo uygulaması üretir. İç, AJAX veri katmanı Deneyimlerle değiştirir, yan yana iki karşılaştırabilmeniz yaklaşıyor. Elbette, Barındırmamıza uygulama potansiyelini şekilleri ilgilendiriyor. Ancak, Barındırmamıza nasıl çalıştığını görürsünüz ve bu geçiş yapmak için nasıl biraz gereklidir.

Haydi başlayalım.

## <a name="create-a-breezeknockout-template-project"></a>Breeze/Knockout şablonu projesi oluşturma

İndirin ve yukarıdaki indir düğmesine tıklayarak şablonu yükleyin. Şablon, Visual Studio Uzantısı (VSIX) dosyası olarak paketlenir. Visual Studio'yu yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Projeyi adlandırın ve tıklayın **Tamam**.

İçinde **yeni proje** seçin **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Derleme ve hata ayıklama olmadan uygulamayı çalıştırmak için CTRL-F5 tuşuna basın veya hata ayıklamayla çalıştırmak için F5 tuşuna basın.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

Doğrulama mantığını Breeze tarafından gerçekleştirilen istemci-tarafı ' dir. Doğrulama öznitelikleri sunucusu model sınıfları istemciye yayılır ve sunucunun istemci irtibata geçmeden önce otomatik olarak yürütülür.

Ağ trafiği gözden geçirin. Meltem bir hata algılandığında sunucuya çağrı olduğunu dikkat edin. Geçerli her değişiklik, bir POST isteğinde "/ Todo/API/SaveChanges" sonuçlandı. Meltem değişiklikleri oluşturur ve Web API denetleyicinin birlikte tek bir istek gönderir `SaveChanges` yöntemi. GÖNDERİN ve her öğe için olan istekleri ayrı ayrı silmek yerine getiren KockoutJS SPA şablondan farklı olmasıdır.

## <a name="peek-inside"></a>İçinde Özet

Bu uygulama, istemci tarafı ve sunucu tarafı sahiptir. Üçüncü taraf JavaScript kitaplıkları ("Komut" klasöründe) artı küçük bir HTML ve uygulama JavaScript modüllerinde ("uygulama" klasöründe) birleşimi, istemci tarafı yığın oluşur.

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

SPA KnockoutJS şablonu araştırılması, bu tanıdık gelecektir. Mavi kutular odaklanın. Model-View-ViewModel (hangi görünümün HTML pencere öğeleri düzgün bir şekilde görünüm modeli destekleyici sunu koddan ayrılmış MVVM), kullanıcı Arabirimi mimaridir. Her işini diğer tutun bilgisi olmadan yapabilir, böylece bir veri bağlama sistemine (Bu durumda Knockout) görünümü ve görünüm modeli düzenler.

Todo veri modeli kapsüller. Görünümünde pencere öğeleri için doğrudan bağlanabilir şekilde modeli'ndeki varlıkları Knockout gözlemlenebilir özelliklerle Breeze tarafından oluşturulur. Görünüm modeli almak ve modeli varlıklarından kaydetmek için veri bağlamı ister. Veri bağlamı Breeze işin çoğunu atar.

Sunucu tarafı yığın Geliştirici kod ve üç ilkesi .NET kitaplıkları oluşur: Web API'si, Entity Framework ve Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Temel mimari KockoutJS SPA şablonu ile aynıdır. Ancak, uygulama çok daha kolaydır: Dto'lar silindi ve Entity Framework ayrıntılarını çoğunu Breeze.NET için yönetici temsilcisi.

## <a name="next-steps"></a>Sonraki Adımlar

Tarafından destekli kod keşfedin öneririz [kapsamlı tartışma](http://www.breezejs.com/spa-template?utm_source=ms-spa) hem istemci hem de sunucu yığınları Breeze Web sitesinde.

Meltem istemci-tarafı sorgu yürütmeyi deneyebilirsiniz; Bazı filtreleri ve sıralamayı ekleyin. Daha fazla model özellikleri ve uçtan uca SPA geliştirme için daha iyi bir genel görünüm almak için daha fazla varlık ekleyebilirsiniz. Tasarımını olduğunuzda Todo özellikleri ayırma ve bunları kendi değerlerinizle değiştirin.

Hemen bir sonraki büyük adım için hazır olacaksınız: istemci tarafı ekranları ekleme ve aralarında gezinme. Bu SPA şablon gerisine bırakmak ve daha eksiksiz bir SPA yığınına gibi kapatma [John Papa'nın Hot Towel](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), Barındırmamıza ve Knockout Karışıma Durandal ekler.
