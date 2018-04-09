---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: '5. Kısım: İş mantığı | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 5 bazı iş mantığı ekler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-5-business-logic"></a>5. Kısım: İş mantığı
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 5 bazı iş mantığı ekler.


## <a id="_Toc260221671"></a>  Bazı iş mantığı ekleme

Birisi web sitemizi ziyaret her kullanılabilmesini alışveriş deneyimi bizim istiyoruz. Ziyaretçilerin göz atın ve bunlar olmayan kaydedilmiş veya oturum açmış olsa bile öğeleri alışveriş sepetine eklemek mümkün olacaktır. Kullanıma hazır olduğunuzda kimliğini doğrulamak için seçeneği sunulur ve değillerse henüz üye bir hesap oluşturmak seçebilecekler.

Başka bir deyişle, biz "Kayıtlı kullanıcı" durumuna anonim bir durumdan alışveriş sepeti dönüştürmek için mantığı uygulamanız gerekir.

Şimdi "Sınıfları" adlı bir dizin oluşturun ardından klasörü sağ tıklatın ve MyShoppingCart.cs adlı yeni bir "Sınıf" dosya oluşturma

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Biz MyShoppingCart.aspx sayfasını uygulayan sınıf genişletme ve bu kullanarak gerçekleştiririz daha önce belirtildiği gibi. NET'in güçlü "kısmi sınıf" oluşturun.

Bizim MyShoppingCart.aspx.cf dosyası için oluşturulan çağrı şuna benzer.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

"Kısmi" anahtar sözcük kullanımına dikkat edin.

Az önce oluşturulan sınıf dosyası şuna benzer.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Bu dosyaya partial anahtar sözcüğü ekleyerek biz bizim uygulamaları birleştirir.

Bizim yeni sınıf dosyası şimdi şöyle görünür.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Bizim sınıfına ekleyeceğiz ilk yöntemi "AddItem" yöntemidir. Bu kullanıcı "için resim ekleme" bağlantılar ürün listesi ve ürün ayrıntıları sayfalarında tıkladığında, sonuçta çağrılacağı yöntemidir.

Kullanarak aşağıdaki append sayfanın üst kısmındaki deyimleri.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

Ve bu yöntem MyShoppingCart sınıfına ekleyin.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Öğe zaten sepetinizde olup olmadığını görmek için LINQ to Entities kullanıyoruz. Bu nedenle, biz miktar öğenin güncelleştirirseniz, aksi takdirde seçili öğe için yeni bir giriş oluşturuyoruz

Bu yöntemi çağırabilmeniz için yalnızca bu yöntem sınıf ancak öğe eklendikten sonra bir = alışveriş geçerli görüntülenen bir AddToCart.aspx sayfası gerçekleştireceksiniz.

Çözüm Gezgini'nde çözüm adına sağ tıklayın ve eklemek ve biz daha önce yaptığınız gibi AddToCart.aspx adlı yeni bir sayfa.

Biz bizim uygulamasında düşük stok sorunları, vb., gibi ara sonuçların görüntülenmesi için bu sayfayı kullanabilirsiniz, ancak sayfa gerçekten işlemek, ancak bunun yerine "Ekle" mantığı çağırın ve yeniden yönlendirme.

Bunu gerçekleştirmek için aşağıdaki kod sayfasına ekleyeceğiz\_yük olay.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Biz bir sorgu dizesi parametresi ve bizim sınıfının addItem yöntemi çağırma alışveriş sepetine eklemek için ürün alınırken unutmayın.

Hiçbir hata varsayılarak karşılaştı denetim tam olarak biz sonraki gerçekleştireceksiniz SHoppingCart.aspx sayfaya geçirilir. Biz bir özel durum bir hata varsa olması gerekir.

Şu anda biz henüz genel hata işleyicisine bu özel durumun uygulamamız tarafından işlenmeyen geçecek ancak Biz bu kısa süre içinde çözebilir uygulanan değil.

Ayrıca Debug.Fail() (aracılığıyla kullanılabilen deyimi kullanımına dikkat edin `using System.Diagnostics;)`

Uygulama içinde hata ayıklayıcı çalışıyor, bu yöntem belirttiğimiz hata iletisi ile birlikte uygulamalarının durumu hakkında bilgi içeren ayrıntılı bir iletişim kutusu görüntüler olur.

Ne zaman Debug.Fail() deyimi üretimde çalışan göz ardı edilir.

Bizim alışveriş sepeti sınıf adları "GetShoppingCartId" yöntemine yapılan bir çağrı yukarıda kodda Not.

Yöntemini aşağıdaki gibi uygulamak için kodu ekleyin.

Ayrıca güncelleştirme ve checkout düğmeleri ve biz "Toplam" Sepeti burada görüntüleyebilirsiniz etiket ekledik olduğunu unutmayın.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Biz şimdi bizim alışveriş sepetine öğeleri ekleyebilir, ancak biz bir ürün eklendikten sonra Sepeti görüntülemek için mantığı uyguladık değil.

Bu nedenle, MyShoppingCart.aspx sayfasında bir EntityDataSource ve GridVire denetimi gibi ekleyeceğiz.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Böylece güncelleştirme Sepeti düğmesine çift tıklayın ve biçimlendirme bildiriminde belirtilen click olay işleyicisi oluşturmak form tasarımcısında yukarı çağırın.

Ayrıntılar daha sonra uygulamanız, ancak bunun yapılması oluşturacak bize ve hatasız uygulamamızı çalıştırmak.

Uygulamayı çalıştırın ve bir öğe alışveriş sepetine eklediğinizde bu görürsünüz.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Biz "varsayılan" kılavuz görüntüden üç özel sütunlar uygulayarak deviated olduğunu unutmayın.

İlk düzenlenebilir, "Bağlı" alanı miktarı için şöyledir:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Sonraki satır (öğesi kez sıralanmalıdır maliyet miktar) öğesi toplam "hesaplanan" sütununu şöyledir:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Son kullanıcının öğesi alışveriş grafikten kaldırılması gerektiğini belirtmek için kullanacağı bir onay kutusu denetimi içeren özel bir sütun vardır.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Gördüğünüz gibi toplam satır sağlandığından boştur sipariş sipariş toplam hesaplamak için bazı mantık ekleyin.

Biz öncelikle "GetTotal" yöntemi bizim MyShoppingCart sınıf için uygulama.

MyShoppingCart.cs dosyasına aşağıdaki kodu ekleyin.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Ardından sayfasında\_biz çağırabilir bizim GetTotal yöntemi yük olay işleyicisi. Aynı anda alışveriş sepeti boş olup olmadığını ve bu görüntü uygun şekilde ayarlamak için bir test ekleyeceğiz.

Alışveriş sepeti boşsa, şimdi Biz bu alın:

![](tailspin-spyworks-part-5/_static/image4.jpg)

Ve aksi durumda, biz bizim toplam bakın.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Ancak, bu sayfa henüz tamamlanmadı.

Alışveriş sepeti kaldırılmak üzere işaretlendi öğeleri kaldırma ve bazı kılavuzda kullanıcı tarafından değiştirilmiş olabilecek gibi yeni miktar değerlerini belirleme tarafından yeniden hesaplamak için ilave bir mantık ihtiyacımız.

Bir kullanıcı bir öğe kaldırma için işaretler olduğunda durumu işlemek için MyShoppingCart.cs bizim alışveriş sepeti sınıfında bir "RemoveItem" yöntem ekleyin olanak sağlar.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Şimdi şimdi ad kullanıcı yalnızca GridView sıralanmalıdır kalite değiştirdiğinde koşullar işlemek için bir yöntem.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Temel özelliklerle Kaldır ve güncelleştirme yerinde gerçekten veritabanında alışveriş sepeti güncelleştirmeleri mantığı uygulayabilirsiniz. (MyShoppingCart.cs içinde)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Bu yöntemi iki parametre beklediğini unutmayın. Bir alışveriş sepeti kimliği ve diğer kullanıcı tanımlı tür nesneler dizisi.

Kullanıcı arabirimi özellikleri hakkında mantığımızı bağımlılık en aza indirmek amacıyla alışveriş sepeti öğeleri için kodumuza bizim yöntemi doğrudan erişim GridView denetimi gerek geçirmek için kullanabileceğiniz bir veri yapısı tanımladığınız.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Bizim MyShoppingCart.aspx.cs dosyasında Biz bu yapı bizim güncelleştirme düğmesini tıklatın olay işleyicisini aşağıdaki gibi kullanabilirsiniz. Sepeti güncelleştirmeye ek olarak da Sepeti toplam güncelleştireceğiz olduğunu unutmayın.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Bu kod satırı ile özellikle ilgisini çeken dikkat edin:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() MyShoppingCart.aspx.cs içinde aşağıdaki gibi uygulayan bir özel yardımcı işlevdir.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Bu bizim GridView denetimini ilişkili öğeleri değerlerine erişmek için temiz bir yol sağlar. Bizim "Öğeyi kaldır" onay kutusu denetimi bağlı değilse bu yana biz FindControl() yöntemiyle eriştiklerini.

Bu aşamada, projenizin geliştirme şu anda kullanıma alma işlemini uygulamak için hazır alınıyor.

Bunu şimdi yaptıktan önce üyelik veritabanı oluşturun ve üyelik depoya bir kullanıcı eklemek için Visual Studio'yu kullanın.

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-4.md)
> [sonraki](tailspin-spyworks-part-6.md)
