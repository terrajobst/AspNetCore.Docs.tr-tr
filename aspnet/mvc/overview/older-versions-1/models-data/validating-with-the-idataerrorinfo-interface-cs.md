---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: IDataErrorInfo arabirimi ile (C#) doğrulanıyor | Microsoft Docs
author: StephenWalther
description: Stephen Walther bir model sınıfı IDataErrorInfo arabirimi uygulayarak özel doğrulama hata iletilerinin görüntülenip gösterilmiştir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>IDataErrorInfo arabirimi ile (C#) doğrulanıyor
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther bir model sınıfı IDataErrorInfo arabirimi uygulayarak özel doğrulama hata iletilerinin görüntülenip gösterilmiştir.


Bu öğretici bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için bir yaklaşım açıklamak için hedefidir. Birisi gerekli form alanları için değerleri sağlamadan bir HTML formuna göndermelerini engellemek öğrenin. Bu öğreticide, IErrorDataInfo arabirimini kullanarak doğrulama gerçekleştirmek nasıl öğrenin.

## <a name="assumptions"></a>Varsayımlar

Bu öğreticide, MoviesDB veritabanı ile filmler veritabanı tablosu kullanmam. Bu tablo şu sütunları vardır:

<a id="0.5_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(100) | False |
| Director | Nvarchar(100) | False |
| DateReleased | DateTime | False |


Bu öğreticide, ı my veritabanı modeli sınıfları oluşturmak için Microsoft Entity Framework kullanın. Entity Framework tarafından oluşturulan film sınıfı Şekil 1'de görüntülenir.


[![Film varlık](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Şekil 01**: film varlık ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Veritabanı modeli sınıfları oluşturmak için Entity Framework kullanma hakkında daha fazla bilgi için Model sınıflarıyla oluşturma Entity Framework my öğretici başlıklı bakın.


## <a name="the-controller-class"></a>Denetleyici sınıfı

Biz listesi filmler giriş denetleyiciye kullanın ve yeni filmler oluşturun. Bu sınıf için kod listeleme 1'de yer alır.

**1 - Controllers\HomeController.cs listeleme**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

Listeleme 1 giriş controller sınıfında iki Create() eylemleri içerir. İlk eylem yeni film oluşturmaya için HTML formu görüntüler. İkinci Create() eylemi veritabanına yeni film gerçek INSERT gerçekleştirir. İkinci Create() eylemi sunucuya ilk Create() eylem tarafından görüntülenen form gönderildiğinde çağrılır.

İkinci Create() eylemi aşağıdaki kod satırlarını içerdiğine dikkat edin:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

Bir doğrulama hatası olduğunda IsValid özelliği false değerini döndürür. Bu durumda, bir filmi oluşturmak için HTML formu içeren Oluştur görünümünün görünürler.

## <a name="creating-a-partial-class"></a>Kısmi bir sınıf oluşturma

Film sınıf Entity Framework tarafından oluşturulur. Çözüm Gezgini penceresinde MoviesDBModel.edmx dosyasını genişletin ve kod düzenleyicisinde MoviesDBModel.Designer.cs dosyasını açın, film sınıfı için kod görebilirsiniz (bkz: Şekil 2).


[![Film varlık için kod](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Şekil 02**: film varlık için kod ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


Film sınıfı kısmi bir sınıftır. Film sınıf işlevselliğini genişletmek için aynı ada sahip başka bir parçalı sınıf ekleyebiliriz anlamına gelir. Yeni sınıfa doğrulama mantığımızı ekleyeceğiz.

Sınıf modeller klasörü listeleme 2'de ekleyin.

**2 - Models\Movie.cs listeleme**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Listeleme 2 sınıfında içeren bildirim *kısmi* değiştiricisi. Herhangi bir yöntemi veya bu sınıfa ekleyin özellikleri Entity Framework tarafından oluşturulan film sınıfı bir parçası haline gelir.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>OnChanging ve OnChanged kısmi yöntemler ekleme

Entity Framework kısmi yöntemler sınıfa Entity Framework bir varlık sınıfı oluşturduğunda, otomatik olarak ekler. Entity Framework sınıfın her bir özellik için karşılık gelen OnChanging ve OnChanged kısmi yöntemler oluşturur.

Film sınıfı söz konusu olduğunda, aşağıdaki yöntemlerden Entity Framework oluşturur:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Karşılık gelen özelliği değiştirilmeden önce OnChanging yöntemi sağ çağrılır. Özellik değiştirildikten sonra OnChanged yöntemi sağ çağrılır.

Doğrulama mantığını film sınıfı eklemek için bu kısmi yöntemlerin avantajından yararlanabilirsiniz. Güncelleştirme film sınıfı listeleme 3 başlık ve Director özellikleri boş olmayan değerler atanır doğrular.

> [!NOTE] 
> 
> Kısmi bir yöntem uygulamak için gerekli olmayan bir sınıf içinde tanımlanan bir yöntemdir. Kısmi bir yöntem uygularsanız yok derleyici yöntem imzası kaldırır ve tüm çağrıları yöntemi yani vardır kısmi yöntemiyle ilişkili çalışma zamanı maliyetleri olmadan aynıdır. Visual Studio Kod Düzenleyicisi'nde anahtar sözcüğü yazarak kısmi bir yöntem ekleyebilirsiniz *kısmi* uygulamak için kısmi bir listesini görüntülemek için bir boşluk bırakarak.


**3 - Models\Movie.cs listeleme**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Başlık özellik boş bir dize atama çalışırsanız, örneğin, daha sonra bir hata iletisi adlı bir sözlük atanır \_hataları.

Bu noktada, hiçbir şey gerçekte başlık özelliği boş bir dize atamak ve bir hata özel eklendiğinde gerçekleşir \_hataları alan. ASP.NET MVC çerçevesi bu doğrulama hatalarını kullanıma sunmak için IDataErrorInfo arabirimi uygulamak gerekir.

## <a name="implementing-the-idataerrorinfo-interface"></a>IDataErrorInfo arabirimi uygulama

IDataErrorInfo arabirimi ilk sürümünden bu yana .NET framework'ün bir parçası olmuştur. Bu arabirim çok basit bir arabirimdir:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Bir sınıf IDataErrorInfo arabirimi uygularsa, ASP.NET MVC çerçevesi sınıfının bir örneğini oluştururken bu arabirimini kullanır. Örneğin, ev denetleyicisi Create() eylem film sınıfının bir örneği kabul eder:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

ASP.NET MVC çerçevesi model bağlayıcı (DefaultModelBinder) kullanarak Create() eyleme geçirilen film örneği oluşturur. Model bağlayıcı Film nesnesini örneğine HTML form alanlarını bağlayarak film nesnesinin örneğini oluşturmaktan sorumludur.

Bir sınıf IDataErrorInfo arabirimi uygulayan olup olmadığına bakılmaksızın DefaultModelBinder algılar. Bir sınıfı bu arabirimi uygular, model bağlayıcı sınıfın her bir özellik için IDataErrorInfo.this dizin oluşturucuyu çağırır. Dizin Oluşturucu bir hata iletisi döndürürse, model bağlayıcı durumu otomatik olarak modellemek için bu hata iletisi ekler.

DefaultModelBinder de IDataErrorInfo.Error özelliğini denetler. Bu özellik ile ilişkili özelliği olmayan belirli doğrulama hatalarını temsil etmek üzere tasarlanmıştır. Örneğin, film sınıfın birden çok özelliklerin değerlerine bağlıdır bir doğrulama kuralı zorlamak isteyebilirsiniz. Bu durumda, hata özelliğinden bir doğrulama hata döndürecektir.

Listeleme 4 güncelleştirilmiş film sınıfında IDataErrorInfo arabirimi uygular.

**4 - Models\Movie.cs (IDataErrorInfo uygulayan) listeleme**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Dizin Oluşturucu özelliği listeleme 4'te denetler \_özellik adına karşılık gelen bir anahtarı içerip içermediğini görmek için hatalar koleksiyonuna geçirilen için dizin oluşturucu. Özellik ile ilişkilendirilmiş doğrulama hata yoksa boş bir dize döndürülür.

Giriş denetleyicisi değiştirilmiş film sınıfını kullanmak için herhangi bir şekilde değiştirmek gerekmez. Şekil 3'te görüntülenen sayfa başlığının ya da Director form alanları için herhangi bir değer girdiğinizde ne olacağını gösterir.


[![Eylem yöntemleri otomatik olarak oluşturma](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Şekil 03**: eksik değerleri bir formla ([tam boyutlu görüntüyü görüntülemek için tıklatın](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


DateReleased değer otomatik olarak doğrulanır dikkat edin. Bir değere sahip olmadığı durumlarda DefaultModelBinder, bu özellik için bir doğrulama hatası DateReleased özelliği NULL değerleri kabul etmiyor olduğundan, otomatik olarak oluşturur. DateReleased özelliği için hata iletisini değiştirmek istiyorsanız özel bir model bağlayıcısını oluşturmanız gerekir.

## <a name="summary"></a>Özet

Bu öğreticide, IDataErrorInfo arabirimi doğrulama hata iletileri oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz. İlk olarak, Entity Framework tarafından oluşturulan kısmi film sınıf işlevselliğini genişleten bir kısmi film sınıfı oluşturduk. Ardından, doğrulama mantığını film sınıfı OnTitleChanging() ve OnDirectorChanging() kısmi yöntemlerine eklediğimiz. Son olarak, biz bu doğrulama iletileri ASP.NET MVC çerçevesi için kullanıma sunmak için IDataErrorInfo arabirimi uygulanmadı.

> [!div class="step-by-step"]
> [Önceki](performing-simple-validation-cs.md)
> [sonraki](validating-with-a-service-layer-cs.md)
