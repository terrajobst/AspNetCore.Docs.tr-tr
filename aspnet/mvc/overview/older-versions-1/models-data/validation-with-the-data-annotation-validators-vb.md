---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Veri ek açıklaması doğrulayıcıları (VB) ile doğrulama | Microsoft Docs
author: microsoft
description: Bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklaması Model bağlayıcı yararlanın. Doğrulayıcı farklı türde kullanmayı öğrenin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: d1987182a44a0ad3f91f455342dc934d1dd50267
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="validation-with-the-data-annotation-validators-vb"></a>Veri ek açıklaması doğrulayıcıları (VB) ile doğrulama
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklaması Model bağlayıcı yararlanın. Doğrulayıcı öznitelikleri farklı türde kullanın ve bunları Microsoft Entity Framework çalışma hakkında bilgi edinin.


Bu öğreticide, veri ek açıklamasını doğrulayıcıları bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için nasıl kullanılacağını öğrenin. Veri ek açıklamasını doğrulayıcıları kullanmanın avantajı, doğrulama gerçekleştirmek yalnızca ekleyerek – gerekli gibi bir veya daha fazla öznitelikleri ya da StringLength öznitelik – bir sınıf özelliği için Etkinleştir ' dir.

Veri ek açıklamasını doğrulayıcıları kullanmadan önce veri ek açıklamaları Model bağlayıcısını yüklemeniz gerekir. Tıklayarak veri ek açıklamaları Model bağlayıcı örneği CodePlex Web sitesinden indirebileceğiniz [burada](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Veri ek açıklamaları Model Bağlayıcısı Microsoft ASP.NET MVC çerçevesi resmi bir parçası olduğunu anlamak önemlidir. Veri ek açıklamaları Model bağlayıcı Microsoft ASP.NET MVC ekibi tarafından oluşturulmuş olsa da, Microsoft açıklandığı ve Bu öğreticide kullanılan veri ek açıklamaları Model bağlayıcı için resmi ürün destek sağlamaz.


## <a name="using-the-data-annotation-model-binder"></a>Veri ek açıklaması Model Bağlayıcısı kullanma

Bir ASP.NET MVC uygulamasındaki veri ek açıklamaları Model bağlayıcısını kullanmak için öncelikle Microsoft.Web.Mvc.DataAnnotations.dll derleme ve System.ComponentModel.DataAnnotations.dll derleme için bir başvuru eklemeniz gerekir. Menü seçeneğini **proje, Başvuru Ekle**. İleri'yi **Gözat** sekmesinde ve indirdiğiniz (ve sıkıştırması açılmış) veri ek açıklamaları Model bağlayıcı örnek konumuna göz atın (bkz **Şekil 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Şekil 1**: veri ek açıklamaları Model bağlayıcı için bir başvuru ekleyerek ([tam boyutlu görüntüyü görüntülemek için tıklatın](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Microsoft.Web.Mvc.DataAnnotations.dll derleme ve System.ComponentModel.DataAnnotations.dll derleme seçip tıklatın **Tamam** düğmesi.


.NET Framework Service Pack 1 veri ek açıklamaları Model Bağlayıcısı ile birlikte gelen System.ComponentModel.DataAnnotations.dll derleme kullanamazsınız. Veri ek açıklamaları Model bağlayıcı örneği indirmeye dahil System.ComponentModel.DataAnnotations.dll derleme sürümünü kullanmanız gerekir.


Son olarak, Global.asax dosyasında DataAnnotations Model bağlayıcı kaydetmeniz gerekir. Uygulamaya aşağıdaki kod satırını ekleyin\_Start() olay işleyicisi böylece uygulama\_Start() yöntemini aşağıdaki gibi görünür:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Bu kod satırı DataAnnotationsModelBinder tüm ASP.NET MVC uygulaması için varsayılan model bağlayıcısını olarak kaydeder.

## <a name="using-the-data-annotation-validator-attributes"></a>Veri ek açıklaması Doğrulayıcı öznitelikleri kullanma

Veri ek açıklamaları Model bağlayıcısını kullandığınızda, doğrulama gerçekleştirmek için Doğrulayıcı öznitelikleri kullanın. System.ComponentModel.DataAnnotations ad alanı, aşağıdaki Doğrulayıcı öznitelikleri içerir:

- Aralık – bir özelliğin değerini belirtilen bir değer aralığının arasında olup olmadığını doğrulamanıza olanak sağlar.
- ReqularExpression – bir özelliğin değerini belirtilen normal ifade deseni ile eşleşip eşleşmediğini doğrulamanıza olanak sağlar.
- Gerekli – bir özellik gerekli olarak işaretlemek sağlar.
- StringLength – bir dize özelliği için maksimum uzunluk belirtmenize olanak sağlar.
- Doğrulama – tüm Doğrulayıcı öznitelikleri için temel sınıf.

> [!NOTE] 
> 
> Doğrulama gereksinimlerinizi herhangi bir standart doğrulayıcıları tarafından değil sağlanırsa sonra her zaman yeni bir doğrulayıcı öznitelik temel doğrulama özniteliğinden devralarak özel Doğrulayıcı özniteliği oluşturma seçeneğiniz vardır.


Product sınıfında **listeleme 1** bu Doğrulayıcı öznitelikleri nasıl kullanıldığını gösterir. Ad, açıklama ve UnitPrice özellikleri işaretlenmiş gerektiği gibi. Name özelliği 10'dan az karakter olan bir dize uzunluğu olması gerekir. Son olarak, UnitPrice özelliği, bir para birimi miktarını temsil eden bir normal ifade deseni eşleşmesi gerekir.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Kod 1**: Models\Product.vb

Ürün sınıfı bir ek özniteliğin nasıl kullanılacağını göstermektedir: DisplayName özniteliği. DisplayName özniteliği özelliği bir hata iletisi görüntülendiğinde özelliğinin adını değiştirmenize olanak sağlar. Hata iletisi "UnitPrice gerekli bir alandır" yerine "Fiyat gerekli bir alandır" hata iletisi görüntüleyebilirsiniz.

> [!NOTE] 
> 
> Tamamen Doğrulayıcısı tarafından görüntülenen hata iletisini özelleştirmek istiyorsanız bu gibi Doğrulayıcısı'nın ErrorMessage özelliği için bir özel hata iletisi atayabilirsiniz: `<Required(ErrorMessage:="This field needs a value!")>`


Product sınıfında kullanabilirsiniz **listeleme 1** Create() denetleyici eylemi ile **listeleme 2**. Model durumu hataları içerdiğinde Bu denetleyici eylemi Oluştur görünümünün görüntüler.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Kod 2**: Controllers\ProductController.vb

Son olarak, görünümünde oluşturabilirsiniz **listeleme 3** Create() eylem sağ tıklayarak ve menü seçeneğini belirleyerek **Görünüm Ekle**. Kesin türü belirtilmiş görünüm model sınıfı ürün sınıfı oluşturun. Seçin **oluşturma** görünümü içerik aşağı açılan listeden (bkz **Şekil 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Şekil 2**: oluşturma görünümü ekleme

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Listing 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Tarafından oluşturulan form oluştur ID alanı kaldırma **Görünüm Ekle** menü seçeneği. ID alanı karşılık gelen bir kimlik sütunu olduğundan, bu alan için bir değer girin yapmalarına izin vermek istemezsiniz.


Ürün oluşturma için form gönderme ve gerekli alanlar için değerleri girmeyin durumunda doğrulama hatası iletilerini **Şekil 3** görüntülenir.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Şekil 3**: gerekli alanlar eksik

Bir geçersiz para birimi miktarını sonra hata iletisinde girerseniz **Şekil 4** görüntülenir.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Şekil 4**: geçersiz bir para birimi

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Veri ek açıklaması doğrulayıcıları Entity Framework ile kullanma

Microsoft Entity Framework veri modeli sınıfları oluşturmak için kullanıyorsanız, sınıflarınızı doğrudan Doğrulayıcı öznitelikleri uygulanamıyor. Entity Framework Designer modeli sınıfları oluşturduğundan, model sınıflarına yaptığınız tüm değişiklikler Tasarımcısı'nda herhangi bir değişiklik bir sonraki sefer üzerine yazılır.

Entity Framework tarafından oluşturulan sınıflar ile doğrulayıcıları kullanmak istiyorsanız, meta veri sınıfları oluşturmak gerekir. Doğrulayıcıları doğrulayıcıları gerçek sınıfı için uygulama yerine meta veri sınıfı için geçerlidir.

Örneğin, Entity Framework kullanarak film sınıf oluşturduğunuzu düşünün (bkz **Şekil 5**). Ayrıca, filmi ve Director özellikleri gerekli özellikleri yapmak istediğiniz düşünün. Bu durumda, kısmi ve meta veri sınıf oluşturabilirsiniz **listeleme 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Şekil 5**: Entity Framework tarafından oluşturulan film sınıfı

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**4 listeleme**: Models\Movie.vb

Dosyada **listeleme 4** film ve MovieMetaData adlı iki sınıflar içerir. Film sınıfı kısmi bir sınıftır. DataModel.Designer.vb dosyasında yer alan Entity Framework tarafından oluşturulan sınıfa karşılık gelir.

Şu anda, .NET framework kısmi özellikleri desteklemiyor. Bu nedenle, Doğrulayıcı öznitelikleri dosyasında tanımlanmış film sınıf özelliklerini uygulayarak DataModel.Designer.vb dosyasında tanımlanan film sınıfının özelliklerine Doğrulayıcı öznitelikleri uygulamak için bir yolu yoktur **4listeleme**.

Film parçalı sınıf MovieMetaData sınıfa işaret eden bir MetadataType özniteliği ile tasarlandığına dikkat edin. MovieMetaData sınıfı film sınıfının özelliklerine proxy özellikleri içerir.

Doğrulayıcı öznitelikleri MovieMetaData sınıfının özelliklerine uygulanır. Başlık, Director ve DateReleased özellikleri tüm gerekli özellikleri olarak işaretlenir. 5'den az karakter içeren bir dizeyi Director özelliği atanması gerekir. Son olarak, DisplayName özniteliği "serbest bırakılmış tarihi gerekli bir alandır."gibi bir hata iletisi görüntülenecek DateReleased özelliğine uygulanır hata yerine "DateReleased alan gereklidir."

> [!NOTE] 
> 
> MovieMetaData sınıfı proxy özelliklerinde film sınıfı karşılık gelen özelliklerinde aynı türlerine temsil gerekmez dikkat edin. Örneğin, Director film sınıfında bir dize özelliği ve MovieMetaData sınıfında bir nesne özelliği özelliğidir.


Sayfanın **Şekil 6** film özelliklerini geçersiz değerler girdiğinizde döndürülen hata iletilerini gösterir.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Şekil 6**: doğrulayıcıları ile Entity Framework kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Özet

Bu öğreticide, bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklaması Model bağlayıcı yararlanmak nasıl öğrendiniz. Gerekli gibi Doğrulayıcı öznitelikleri ve StringLength öznitelikleri farklı türleri kullanmayı öğrendiniz. Ayrıca Microsoft Entity Framework ile çalışırken, bu öznitelikler kullanma hakkında bilgi edindiniz.

> [!div class="step-by-step"]
> [Önceki](validating-with-a-service-layer-vb.md)
