---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '5. Bölüm: İş mantığı | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 5. Bölüm bazı iş mantığı ekler.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e18acb66dbdb3bd3e0dfa21193f617dad82afc74
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755666"
---
<a name="part-5-business-logic"></a>5. Bölüm: İş mantığı
====================
tarafından [ALi Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir. Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 5. Bölüm bazı iş mantığı ekler.


## <a id="_Toc260221671"></a>  Bazı iş mantığı ekleme

Alışveriş deneyimimizi birisi web sitemizi ziyaret ne zaman kullanılabilir olmasını istiyoruz. Ziyaretçi göz atın ve bunlar katılmamaları kayıtlı veya oturum açmış olsanız bile, alışveriş sepetine öğe ekleme mümkün olacaktır. Kullanıma hazır olduğunuzda kimliğini doğrulama seçeneği sunulur ve değillerse henüz üyeleri, bir hesap oluşturmak mümkün olacaktır.

Başka bir deyişle, biz alışveriş sepetini anonim bir durumdan bir "Kullanıcı kayıtlı" duruma dönüştürmek için mantığı uygulamak gerekir.

Şimdi "Sınıfları" adlı bir dizin oluşturmak ardından klasörüne sağ tıklayıp MyShoppingCart.cs adlı yeni bir "Class" dosya oluşturma

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Daha önceden belirtildiği biz MyShoppingCart.aspx sayfası uygulayan sınıf genişletme ve bu kullanarak yapacağız. NET güçlü "kısmi Class" yapısı.

Bizim MyShoppingCart.aspx.cf dosya için oluşturulan çağrı aşağıdaki gibi görünür.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

"Kısmi" anahtar sözcüğü kullanımına dikkat edin.

Az önce oluşturulan sınıf dosyası şöyle görünür.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Partial anahtar sözcüğü bu dosyaya ekleyerek size sunduğumuz uygulamaları birleştirecektir.

Sunduğumuz yeni bir sınıf dosyası artık şöyle görünür.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Bizim sınıfına ekleyeceğiz ilk yöntem "AddItem" yöntemdir. Ürün listesi ve ürün ayrıntıları sayfalarında "Resim Ekle" bağlantılarında kullanıcı tıkladığında, sonuçta çağrılacak yöntem budur.

Aşağıdakileri kullanarak ekleme deyimini sayfasının üst.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Ve bu yöntem MyShoppingCart sınıfına ekleyin.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Öğenin zaten arabasında olup olmadığını görmek için LINQ to Entities kullanıyoruz. Bu nedenle, miktar öğesinin güncelleştirmemiz durumunda, aksi takdirde seçili öğe için yeni bir giriş oluştururuz

Bu yöntem çağırmak için yalnızca bu yöntem sınıf ancak ardından öğesi eklendikten sonra bir = alışveriş geçerli görüntülenen bir AddToCart.aspx sayfası uygular.

Çözüm Gezgini'nde çözümün adına sağ tıklayın ve eklemek ve daha önce uyguladığımız olarak AddToCart.aspx adlı yeni bir sayfa.

Bu sayfada kararlılığımızın içinde düşük stok sorunları, vb., geçiş sonuçları görüntülemek için kullanabiliriz ancak sayfa gerçekten işlemek, ancak bunun yerine "Ekle" mantıksal çağrı ve yeniden yönlendirme.

Bunu gerçekleştirmek için aşağıdaki kod sayfasına ekleyeceğiz\_yükleme olayı.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Biz bir sorgu dizesi parametresini ve bizim sınıfının addItem yöntemi çağırma alışveriş sepetine eklemek için ürün almakta olduğunu unutmayın.

Hiçbir hata olmadığı kabul edilerek karşılaştı denetimi tamamen size sonraki uygulayacak SHoppingCart.aspx sayfasına geçirilir. Biz, bir hata yoksa bir özel durum.

Şu anda henüz genel hata işleyicisi bu özel durumun uygulamamız tarafından işlenmemiş gitmesi gerekiyordu ancak Biz bu kısa bir süre içinde çözebilir uyguladık değil.

Ayrıca Debug.Fail() (aracılığıyla kullanılabilir deyimi kullanımına dikkat edin `using System.Diagnostics;)`

Olan uygulamayı hata ayıklayıcısı içinde çalışırken, bu yöntem belirttiğimiz hata iletisi ile birlikte uygulama durumu hakkındaki bilgileri içeren ayrıntılı bir iletişim kutusu görüntülenir.

Üretimde deyimi yoksayıldı Debug.Fail() çalışırken.

Bizim alışveriş sepeti sınıf adları "GetShoppingCartId" bir yönteme bir çağrı üzerindeki kodda fark edeceksiniz.

Yöntemi gibi uygulamak için kodu ekleyin.

Ayrıca güncelleştirme ve kullanıma alma düğmeleri ve biz "Toplam" sepete burada görüntüleyebilirsiniz etiket ekledik olduğunu unutmayın.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Biz artık alışveriş sepetimizi öğeleri ekleyebilir, ancak bir ürün eklendikten sonra sepet görüntülenecek mantıksal uyguladık değil.

Bu nedenle, MyShoppingCart.aspx sayfasında EntityDataSource denetimini ve GridVire gibi ekleyeceğiz.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Form Tasarımcısı'nda ' kurmak alışveriş sepetini güncelleştir düğmesine çift tıklayın ve biçimlendirme bildiriminde belirtilen click olay işleyicisi oluşturmak için çağırın.

Ayrıntılar daha sonra uygulama ancak bunun yapılması oluşturacak bize ve hatasız uygulamamızı çalıştırmak.

Uygulamayı çalıştırmak ve alışveriş sepetine öğe ekleme görürsünüz.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Biz "varsayılan" kılavuz görüntüden üç özel sütunlar uygulayarak deviated olduğunu unutmayın.

İlk bir düzenlenebilir, "Bağlı" alanı miktarı için verilmiştir:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Sonraki satır (sipariş edilmesi gereken miktar kez öğesi maliyet) öğesi toplam "hesaplanan" sütununu şöyledir:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Son kullanıcının öğe alışveriş şemasından kaldırılması gerektiğini belirtmek için kullanacağı bir CheckBox denetimi içeren özel bir sütun vardır.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Gördüğünüz gibi toplam satır şimdi boş siparişin sipariş toplam hesaplamak için bazı mantık ekleyin.

Bir "GetTotal" yöntem ilk bizim MyShoppingCart sınıfına uygulayacaksınız.

MyShoppingCart.cs dosyasına aşağıdaki kodu ekleyin.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Ardından sayfasında\_Load olay işleyicisinde biz çağırabilir bizim GetTotal yöntemi. Aynı anda alışveriş sepetini boş olup olmadığını ve bu görüntü uygun şekilde ayarlamak için bir test ekleyeceğiz.

Alışveriş sepeti boşsa, artık bu aldığımız:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Ve aksi takdirde, bizim toplam görüyoruz.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Ancak, bu sayfa henüz tamamlanmadı.

Alışveriş sepeti kaldırılmak üzere işaretlendi öğeleri kaldırarak ve bazı kılavuzda kullanıcı tarafından değiştirilmiş olabilecek yeni miktar değerlerini belirleyen ile yeniden hesaplamak için ilave bir mantık ihtiyacımız.

Bir kullanıcı bir öğeyi kaldırma işaretler, durumu işlemek için MyShoppingCart.cs bizim alışveriş sepeti sınıfında "da removeItem" yöntemi eklemek olanak tanır.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Şimdi github'dan bir kullanıcı yalnızca içinde GridView sıralanmalıdır kalitesi değiştiğinde durumda işlemek için bir yöntem ad.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Temel özelliklerle Kaldır ve güncelleştirme yerinde gerçekten veritabanında alışveriş sepetini güncelleştirme mantığı uygulayabilir. (MyShoppingCart.cs içinde)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Bu yöntem, iki parametre bekliyor unutmayın. Bir alışveriş sepeti kimliği ve diğer kullanıcı tanımlı türü nesnelerin dizisi.

Bağımlılık bizim mantığı kullanıcı arabirimi özellikleri hakkında en aza indirmek için alışveriş sepetine öğe kodumuz için sunduğumuz yöntemi GridView denetiminde doğrudan bağlanmasına gerek geçirmek için kullanabileceğiniz bir veri yapısı tanımladınız.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Bizim MyShoppingCart.aspx.cs dosyasında bu yapı bizim güncelleştirme düğmesine tıklayın olay işleyicisi aşağıdaki gibi kullanabiliriz. Alışveriş sepetini güncelleştirme yanı sıra biz de sepet toplam güncelleştirileceğini unutmayın.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Bu kod satırı ile yakından unutmayın:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() MyShoppingCart.aspx.cs içinde şu şekilde uygulayan bir özel yardımcı işlevdir.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Bu, bizim GridView denetimindeki ilişkili öğe değerlerini erişmek için temiz bir yol sağlar. Bizim "Öğeyi kaldır" onay kutusu denetimi bağlı değilse bu yana biz FindControl() yöntemi erişirsiniz.

Bu, projenizin geliştirme aşamasında size kullanıma alma işlemini uygulamak hazır hazırlanıyoruz.

Şimdi bunu önce üyelik veritabanı oluşturun ve üyelik depoya bir kullanıcı eklemek için Visual Studio'yu kullanın.

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-4.md)
> [İleri](tailspin-spyworks-part-6.md)
