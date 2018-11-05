---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Model bağlama ve web formlarını kullanan bir proje için iş mantığı katmanı ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama veri etkileşimi daha fazla düz - sağlar...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 4ba1830ce51f257e18880f752b249dbeb77f504e
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021657"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Model bağlama ve web formlarını kullanan bir proje için iş mantığı katmanı ekleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğretici serisinde, model bağlama kullanarak bir ASP.NET Web formları projesi ile temel yönlerini gösterir. Model bağlama, daha doğru verilerle ilgili kaynak nesne (örneğin, ObjectDataSource veya SqlDataSource) daha veri etkileşim sağlar. Bu seri, tanıtım malzemeleri ile başlar ve sonraki öğreticilerde için daha gelişmiş kavramlar taşır.
> 
> Bu öğreticide, model bağlama iş mantığı katmanı ile kullanmak gösterilmektedir. Geçerli sayfanın dışında bir nesne veri yöntemleri çağırmak için kullanıldığını belirtmek için OnCallingDataMethods üye ayarlanır.
> 
> Bu öğreticide oluşturulan proje geliştirir [önceki](retrieving-data.md) dizisinin bölümleri.
> 
> Yapabilecekleriniz [indirme](https://go.microsoft.com/fwlink/?LinkId=286116) tam projeyi C# veya vb İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablonundan biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Derleme

Model bağlama, veri etkileşimi kodunuzu bir web sayfası için arka plan kod dosyasında veya ayrı iş mantığı sınıfı yerleştirilmesine olanak sağlar. Önceki öğreticilerde, arka plan kod dosyaları için veri etkileşim kodu kullanma göstermiştir. Bu yaklaşım küçük siteler için çalışır ancak büyük bir site bakımı yapılırken kod yinelemesi ve büyük zorluk açabilir. Ayrıca programlı olarak Soyutlama Katmanı olduğundan, arka plan kod dosyalarını bulunan kodu test etmek oldukça zor olabilir.

Veri etkileşim kodu merkezileştirmek için tüm verilerle etkileşim kurmak için mantığı içeren bir iş mantığı katmanı oluşturabilirsiniz. Ardından web sayfalarınızdan iş mantığı katmanı çağırın. Bu öğreticide, tüm iş mantığı katmanı içine önceki öğreticilerde yazdığınız kodun taşıyın ve sonra bu kodu sayfalarından gösterilmektedir.

Bu öğreticide, gerekir:

1. Kod, iş mantığı katmanı için arka plan kod dosyaları Taşı
2. Değişiklik iş mantığı katmanı yöntemleri çağırmak için veri bağlama denetimleri

## <a name="create-business-logic-layer"></a>İş mantığı katmanı oluşturma

Şimdi web sayfalarından adlı bir sınıf oluşturur. Bu sınıftaki yöntemlerin önceki öğreticilerdeki kullanılan yöntemlere benzer ve değer sağlayıcısı özellikleri içerir.

İlk olarak, adlı yeni bir klasör ekleyin **BLL**.

![Klasör Ekle](adding-business-logic-layer/_static/image1.png)

BLL klasöründe adlı yeni bir sınıf oluşturun **SchoolBL.cs**. Tüm arka plan kod dosyalarında başlangıçta belgeler veri işlemlerini içerecektir. Yöntemleri neredeyse arka plan kod dosyasındaki yöntemleri ile aynıdır, ancak bazı değişiklikler içerir.

Artık kod örneği içinde yürütülen dikkat edilecek en önemli bir değişiklik olduğunu **sayfa** sınıfı. Sayfa sınıfını içeren **TryUpdateModel** yöntemi ve **ModelState** özelliği. Bu kod, iş mantığı katmanı için taşındığında, bu üyeleri çağırmak için sayfa sınıfının bir örneği artık yok. Bu sorunu aşmak için eklemelisiniz bir **ModelMethodContext** TryUpdateModel veya ModelState erişen herhangi bir yönteme parametre. TryUpdateModel arayın veya ModelState almak için bu ModelMethodContext parametresini kullanın. Bu yeni bir parametre için hesap için bu web sayfasında herhangi bir ayarı değiştirmek gerekmez.

SchoolBL.cs kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>İş mantığı katmanından veri almak için var olan sayfaları gözden geçirin

Son olarak, sorgular kullanarak iş mantığı katmanı kullanarak arka plan kod dosyasında Students.aspx AddStudent.aspx ve Courses.aspx sayfaları dönüştürülecektir.

Öğrenciler, AddStudent ve kurslar için arka plan kod dosyalarında silin veya açıklama olarak aşağıdaki sorgu metotları:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Veri işlemleri ilgili arka plan kod dosyasındaki kod şimdi olmalıdır.

**OnCallingDataMethods** olay işleyicisi veri yöntemleri için kullanılacak bir nesne belirtmenize imkan tanır. Students.aspx, bu olay işleyicisi için bir değer ekleyin ve iş mantığı sınıftaki yöntemlerin adlarına veri yöntemlerin adlarını değiştirin.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Students.aspx arka plan kod dosyasında CallingDataMethods olayı için olay işleyicisini tanımlar. Bu olay işleyicisinde veri işlemleri için iş mantığı sınıf belirtin.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

AddStudent.aspx benzer değişiklikler yapın.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Courses.aspx benzer değişiklikler yapın.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Uygulamayı çalıştırmak ve daha önce olduğu gibi tüm sayfaları işlev dikkat edin. Doğrulama mantığını ayrıca düzgün çalışır.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, bir veri erişim katmanı ve iş mantığı katmanı kullanmak için uygulamanızı yeniden yapılandırılmış. Belirttiğiniz veri denetimleri veri işlemleri için geçerli sayfa değil bir nesne kullanın.

> [!div class="step-by-step"]
> [Önceki](using-query-string-values-to-retrieve-data.md)
