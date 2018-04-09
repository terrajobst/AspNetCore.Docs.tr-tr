---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Model bağlama ve web forms kullanan projesine ekleniyor iş mantığı katmanı | Microsoft Docs
author: tfitzmac
description: Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha fazla düz - sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Model bağlama ve web forms kullanan projesine ekleniyor iş mantığı katmanı
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici seri model bağlama kullanarak bir ASP.NET Web Forms projesi ile temel yönlerini gösterir. Model bağlama verileri etkileşim daha düz iletme ile veri kaynağı nesneleri (örneğin, ObjectDataSource veya SqlDataSource) ilgilenme daha kolaylaştırır. Bu seri tanıtım malzemeleri ile başlar ve sonraki öğreticileri, daha gelişmiş kavramları taşır.
> 
> Bu öğretici, bir iş mantığı katmanı ile model bağlama kullanmayı gösterir. Geçerli sayfa dışındaki bir nesnenin veri yöntemleri çağırmak için kullanıldığını belirtmek için OnCallingDataMethods üye kümesi.
> 
> Bu öğreticide oluşturduğunuz proje inşa edilmiştir [önceki](retrieving-data.md) seri bölümlerini.
> 
> Yapabilecekleriniz [karşıdan](https://go.microsoft.com/fwlink/?LinkId=286116) C# veya vb tamamlanmış projede İndirilebilir kod, Visual Studio 2012 veya Visual Studio 2013 ile çalışır. Bu öğreticide gösterilen Visual Studio 2013 şablon biraz farklıdır Visual Studio 2012 şablonu kullanır.


## <a name="what-youll-build"></a>Yapı

Model bağlama, veri etkileşim kodunuzu bir web sayfası için arka plan kod dosyasına veya ayrı iş mantığı sınıfında yerleştirilmesine olanak sağlar. Önceki öğreticileri için veri etkileşim kodu arka plan kod dosyaları kullanmayı göstermiştir. Küçük siteler için bu yaklaşım çalışır ancak büyük site bakımı yapılırken kod yineleme ve büyük zorluk açabilir. Ayrıca program aracılığıyla bir soyutlama katmanı olduğundan, dosyaları arkasındaki kodda bulunan kodu test etmek oldukça zor olabilir.

Veri etkileşim kodu merkezileştirmek için tüm veriler ile etkileşim için mantığı içeren bir iş mantığı katmanı oluşturabilirsiniz. Ardından, web sayfalarından iş mantığı katmanı çağırın. Bu öğretici, tüm önceki öğreticiler bir iş mantığı katmanı içine yazılmış Code taşıyın ve sonra bu kodun sayfalarından gösterilmektedir.

Bu öğreticide, gerekir:

1. Kodu arka plan kodu dosyalarından bir iş mantığı katmanı taşıyın.
2. Değişiklik iş mantığı katmanı yöntemleri çağırmak için veri bağlama denetimleri

## <a name="create-business-logic-layer"></a>İş mantığı katmanı oluşturma

Şimdi, web sayfaları adlı sınıfı oluşturur. Bu sınıftaki yöntemlerin önceki eğitimlerine kullanılan yöntemlere benzer ve değer sağlayıcı öznitelikleri içerir.

İlk olarak, adlı yeni bir klasör ekleyin **BLL**.

![klasör ekleme](adding-business-logic-layer/_static/image1.png)

BLL klasöründe adlı yeni bir sınıf oluşturun **SchoolBL.cs**. İlk olarak arka plan kod dosyalarında belgeler veri işlemlerin içerecektir. Yöntemleri neredeyse arka plan kod dosyasına yöntemleri aynıdır, ancak bazı değişiklikler dahil edilir.

Not etmek için en önemli koddan örneği içinde artık yürütüldüğünü değişikliktir **sayfa** sınıfı. Sayfa sınıfı içeren **TryUpdateModel** yöntemi ve **ModelState** özelliği. Bu kod bir iş mantığı katmanı taşındığında, bu üyeler çağırmak için sayfa sınıfının bir örneği artık yok. Bu sorunu çözmek almak için eklemelisiniz bir **ModelMethodContext** TryUpdateModel veya ModelState erişen herhangi bir yönteme parametre. TryUpdateModel arayın veya ModelState almak için bu ModelMethodContext parametresini kullanın. Bu yeni parametre için hesap için web sayfasındaki değişikliği gerekmez.

SchoolBL.cs kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>İş mantığı katmanından verileri almak için var olan sayfaları gözden geçirin

Son olarak, iş mantığı katmanı kullanarak için arka plan kod dosyasına sorgularını kullanarak Students.aspx, AddStudent.aspx ve Courses.aspx sayfaları dönüştürür.

Öğrenciler, AddStudent ve kurslar için arka plan kod dosyalarında silin veya aşağıdaki sorgu metotları Açıklama:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Veri işlemleriyle ilgili arka plan kod dosyasına hiçbir kod artık olmalıdır.

**OnCallingDataMethods** olay işleyicisi için veri yöntemleri kullanmak için bir nesne belirtmenize olanak sağlar. Students.aspx, bu olay işleyicisi için bir değer ekleyin ve veri yöntemlerin adlarını iş mantığı sınıftaki yöntemlerin adlarını değiştirin.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Students.aspx arka plan kod dosyasına CallingDataMethods olay için olay işleyicisini tanımlar. Bu olay işleyicisi veri işlemleri için iş mantığı sınıf belirtin.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

AddStudent.aspx içinde benzer değişiklikleri yapın.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Courses.aspx içinde benzer değişiklikleri yapın.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Uygulamayı çalıştırın ve daha önce sahip oldukları gibi tüm sayfaları işlev dikkat edin. Doğrulama mantığını de düzgün çalışır.

## <a name="conclusion"></a>Sonuç

Bu öğreticide, bir veri erişim katmanı ve iş mantığı katmanı kullanmak için uygulamanızı yeniden yapılandırılmış. Veri denetimleri veri işlemleri için geçerli sayfa değil bir nesne kullanmak belirttiniz.

> [!div class="step-by-step"]
> [Önceki](using-query-string-values-to-retrieve-data.md)
