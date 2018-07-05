---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'İlk ASP.NET MVC ile EF veritabanında: bir görünümü özelleştirme | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: bfbcfd39dd1cf0abe89a00d2958ca010f0e5e109
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376504"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>İlk ASP.NET MVC ile EF veritabanında: bir görünümünü özelleştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework ve ASP.NET iskeleti oluşturma kullanarak mevcut bir veritabanı için bir arabirim sunan bir web uygulaması oluşturabilirsiniz. Bu öğretici serisinde, otomatik olarak kullanıcıların görüntüleme, düzenleme, oluşturma olanak sağlayan bir kod oluşturmak ve bir veritabanı tablosu, bulunan verileri silmek gösterilir. Oluşturulan kod, veritabanı tablosundaki sütunlara karşılık gelir.
> 
> Bu serinin sunu artırmak için otomatik olarak oluşturulan görünümler değişen odaklanır.


## <a name="add-enrolled-courses-to-student-details"></a>Kayıtlı kursları Öğrenci ayrıntıları ekleyin

Oluşturulan kod, uygulamanız için iyi bir başlangıç noktası sağlar, ancak bunu mutlaka tüm uygulamanızda ihtiyacınız olan işlevleri sağlamaz. Kod, uygulamanızın belirli gereksinimlerini karşılayacak şekilde özelleştirebilirsiniz. Şu anda, uygulamanız için seçilen Öğrenci kayıtlı kursları görüntülemez. Bu bölümde, her öğrencinin için için kayıtlı kursları ekleyeceksiniz **ayrıntıları** Öğrenci için görünümü.

Açık **Students/Details.cshtml**ve son aşağıda &lt;/dl&gt; sekmesi, ancak kapatmadan önce &lt;/div&gt; etiketi, aşağıdaki kodu ekleyin.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Bu kod, her kayıt için bir satır için seçilen Öğrenci kayıt tabloda görüntüleyen bir tablo oluşturur. **Görünen** yöntemi HTML ifadeyi temsil eden nesne için (ModelItem) oluşturur. Görüntüleme yöntemi kullanın (yerine yalnızca özellik değeri, kod ekleme) değeri, doğru türü ve şablon türüne göre biçimlendirildiğinde emin olmak için. Bu örnekte, her ifade tek bir özellik döngüsünde geçerli kaydı döndürür ve metin olarak işlenir, ilkel türler değerlerdir.

Öğrenciler/dizin görünüme tekrar göz atın ve seçim **ayrıntıları** Öğrenciler biri için. Kayıtlı kursları Görünümü'nde dahil edilmiş görürsünüz.

![Öğrenci kaydı](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Önceki](changing-the-database.md)
> [İleri](enhancing-data-validation.md)
