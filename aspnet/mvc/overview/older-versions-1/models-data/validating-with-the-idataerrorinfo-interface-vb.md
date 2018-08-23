---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: IDataErrorInfo arabirimi ile doğrulama (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther nasıl bir model sınıfında IDataErrorInfo arabirimi uygulayarak özel doğrulama hatası iletilerinin görüntüleneceğini gösterir.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: e9b989d0110c3947583fd70bd38b29dcb2bb5c31
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753572"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>IDataErrorInfo arabirimi ile doğrulama (VB)
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther nasıl bir model sınıfında IDataErrorInfo arabirimi uygulayarak özel doğrulama hatası iletilerinin görüntüleneceğini gösterir.


Bu öğreticide bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için bir yaklaşım açıklamak için hedefidir. Birisi bir HTML formuna gerekli form alanları için değerler sağlamadan göndermesinin önlenmesine öğrenin. Bu öğreticide, IErrorDataInfo arabirimini kullanarak doğrulama gerçekleştirme konusunda bilgi edinin.

## <a name="assumptions"></a>Varsayımlar

Bu öğreticide, MoviesDB veritabanı ile film veritabanı tablosu kullanacağım. Bu tabloda aşağıdaki sütunları içerir:

<a id="0.6_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | nvarchar(100) | False |
| Direktörü | nvarchar(100) | False |
| DateReleased | DateTime | False |


Bu öğreticide, Microsoft Entity Framework my veritabanı modeli sınıfları oluşturmak için kullanıyorum. Entity Framework tarafından oluşturulan film sınıfı, Şekil 1'de görüntülenir.


[![Film varlık](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Şekil 01**: film varlık ([tam boyutlu görüntüyü görmek için tıklatın](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Veritabanı modeli sınıfları oluşturmak için Entity Framework kullanma hakkında daha fazla bilgi için Entity Framework ile Model sınıfları oluşturma my öğretici başlıklı bakın.


## <a name="the-controller-class"></a>Denetleyici sınıfı

Biz listesi filmler giriş denetleyicisine kullanın ve yeni filmler oluşturun. Bu sınıfın kodu, listeleme 1'de yer alır.

**1 - Controllers\HomeController.vb listeleme**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

Listeleme 1 giriş controller sınıfında iki Create() eylemleri içerir. İlk eylem yeni bir film oluşturmak için HTML form görüntüler. İkinci Create() eylemi, veritabanına yeni filmin gerçek ekleme gerçekleştirir. İkinci Create() eylemi sunucuya ilk Create() eylemi tarafından görüntülenen form gönderildiğinde çağrılır.

İkinci Create() eylem aşağıdaki kod satırlarını içerdiğine dikkat edin:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

IsValid özelliği, bir doğrulama hatası olduğunda false döndürür. Bu durumda, bir filmi oluşturmaya HTML formu içeren Oluştur görünümünün görünürler.

## <a name="creating-a-partial-class"></a>Kısmi bir sınıf oluşturma

Film sınıfı, Entity Framework tarafından oluşturulur. Çözüm Gezgini penceresinde MoviesDBModel.edmx dosyasını genişletin ve MoviesDBModel.Designer.vb dosyasını Kod düzenleyicisinde açın, film sınıfın kodu görebilirsiniz (bkz: Şekil 2).


[![Film varlık için kod](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Şekil 02**: film varlık kodunu ([tam boyutlu görüntüyü görmek için tıklatın](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


Kısmi bir sınıf film sınıftır. Film sınıf işlevlerini genişletmek için aynı ada sahip başka bir kısmi sınıf ekleyebiliriz anlamına gelir. Yeni kısmi sınıfa bizim Doğrulama mantığı ekleyeceğiz.

Sınıf listesi 2'de modelleri klasöre ekleyin.

**2 - Models\Movie.vb listeleme**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Listeleme 2 sınıfı içeren bildirim *kısmi* değiştiricisi. Herhangi bir yöntem ya da bu sınıfa eklediğiniz özellikleri Entity Framework tarafından oluşturulan film sınıfı bir parçası haline gelir.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>OnChanging ve OnChanged kısmi yöntemler ekleme

Entity Framework kısmi yöntemler sınıf için Entity Framework bir varlık sınıfı oluşturduğunda, otomatik olarak ekler. Entity Framework sınıfının her özelliği için karşılık gelen OnChanging ve OnChanged kısmi yöntemler oluşturur.

Film sınıfı söz konusu olduğunda, Entity Framework, aşağıdaki yöntemlerden oluşturur:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Karşılık gelen özelliği değiştirilmeden önce doğru OnChanging yöntemi de çağrılır. Özellik değiştirildikten sonra doğru OnChanged yöntemi de çağrılır.

Doğrulama mantığını film sınıfa eklemek için bu kısmi yöntemlerin yararlanabilirsiniz. Güncelleştirme 3 listeleme film sınıfta, başlık ve Direktörü özellikleri boş olmayan değerler atanır doğrular.

> [!NOTE] 
> 
> Kısmi bir yöntem uygulamak için gerekli olmayan bir sınıf içinde tanımlanan bir yöntemdir. Kısmi bir yöntemin uygulamayıp varsa derleyici metot imzasını kaldırır ve burada yöntemine yönelik tüm çağrılar kısmi yöntemiyle ilişkili hiçbir çalışma zamanı maliyetleri aşağıda sunulmuştur. Visual Studio Kod Düzenleyicisi'nde, anahtar sözcüğü yazarak kısmi bir yöntemin ekleyebilirsiniz *kısmi* uygulamak için kısmi bir listesini görüntülemek için bir boşluk.


**3 - Models\Movie.vb listeleme**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Boş bir dize başlık özelliğini atamayı denerseniz, örneğin, daha sonra bir hata iletisi adlı bir sözlük atanır \_hataları.

Bu noktada, hiçbir şey gerçekten boş bir dize başlık özelliğine atayın ve özel bir hata eklendiğinde gerçekleşir \_hataları alan. IDataErrorInfo arabirimi, ASP.NET MVC çerçevesi şu doğrulama hataları ortaya çıkarmak için uygulanması gerekir.

## <a name="implementing-the-idataerrorinfo-interface"></a>IDataErrorInfo arabirimi uygulama

IDataErrorInfo arabirimi ilk sürümünden bu yana .NET framework'ün bir parçası olmuştur. Bu arabirim çok basit bir arabirimdir:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

IDataErrorInfo arabirimi arabirimini uygulayan bir sınıf, ASP.NET MVC çerçevesi sınıfının bir örneğini oluştururken bu arabirimini kullanır. Örneğin, giriş denetleyicisine Create() eylem film sınıfının bir örneği kabul eder:

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

ASP.NET MVC çerçevesi model bağlayıcı (DefaultModelBinder) kullanarak Create() eyleme geçirilen film örneğini oluşturur. Model bağlayıcı film nesne örneğine HTML form alanlarını bağlayarak film nesnesinin bir örneğini oluşturmak için sorumludur.

IDataErrorInfo arabirimi arabirimini uygulayan bir sınıf olup olmadığını DefaultModelBinder algılar. Bir sınıf bu arabirim uygularsa model bağlayıcı sınıfın her bir özellik için IDataErrorInfo.this dizin oluşturucuyu çağırır. Dizin Oluşturucu bir hata mesajı döndürür, model bağlayıcı durumu otomatik olarak model oluşturmak için bu hata iletisi ekler.

DefaultModelBinder IDataErrorInfo.Error özelliğini de denetler. Bu özellik, sınıf ile ilişkili özel doğrulama özelliği olmayan hataları temsil etmek üzere tasarlanmıştır. Örneğin, film sınıfın birden çok özellik değerlerine bağlı bir doğrulama kuralını uygulamak isteyebilirsiniz. Bu durumda, bir doğrulama hatası hata özelliğinden döndürecektir.

Güncelleştirilmiş film sınıfı listeleme 4'te IDataErrorInfo arabirimi uygular.

**4 - Models\Movie.vb (IDataErrorInfo uygular) listeleme**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

Dizin Oluşturucu özelliği listeleme 4'te denetler \_özellik adına karşılık gelen bir anahtarı içerip içermediğini görmek için hatalar koleksiyonuna Dizin oluşturucuya geçirilen. Özellikle ilişkili hiçbir doğrulama hatası varsa, daha sonra boş bir dize döndürülür.

Giriş denetleyicisine değiştirilmiş film sınıfını kullanmak için herhangi bir şekilde değiştirmeniz gerekmez. Şekil 3'te görüntülenen sayfa başlığı veya yönetmenin form alanları için hiçbir değer girildiğinde ne olacağını gösterir.


[![Eylem yöntemlerine otomatik olarak oluşturma](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Şekil 03**: eksik değerleri bir form ([tam boyutlu görüntüyü görmek için tıklatın](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


DateReleased değeri otomatik olarak doğrulanır dikkat edin. Bir değer olmadığında DefaultModelBinder, bu özellik için bir doğrulama hatası DateReleased özelliği NULL değer kabul etmez olduğundan, otomatik olarak oluşturur. Ardından DateReleased özelliği için hata iletisini değiştirmek istiyorsanız, özel bir model bağlayıcı oluşturmanız gerekir.

## <a name="summary"></a>Özet

Bu öğreticide, doğrulama hatası iletilerini oluşturmak için IDataErrorInfo arabirimi kullanmayı öğrendiniz. İlk olarak, Entity Framework tarafından üretilen kısmi film sınıf işlevlerini genişleten bir kısmi film sınıfı oluşturduk. Ardından, doğrulama mantığı film sınıfı OnTitleChanging() ve OnDirectorChanging() kısmi yöntemlere ekledik. Son olarak, IDataErrorInfo arabirimi ASP.NET MVC çerçevesi bu doğrulama iletileri kullanıma sunmak için uyguladık.

> [!div class="step-by-step"]
> [Önceki](performing-simple-validation-vb.md)
> [İleri](validating-with-a-service-layer-vb.md)
