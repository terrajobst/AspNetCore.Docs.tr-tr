---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF veritabanıyla ilk ASP.NET MVC: bir görünümü özelleştirme | Microsoft Docs'
author: tfitzmac
description: ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF veritabanıyla ilk ASP.NET MVC: bir görünümü özelleştirme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> ASP.NET yapı İskelesi MVC ve Entity Framework kullanarak, varolan bir veritabanını bir arabirim sağlayan bir web uygulaması oluşturabilirsiniz. Bu öğretici seri otomatik olarak görüntüleme, düzenleme, oluşturmak kullanıcıların sağlayan kodu oluşturmak ve veritabanı tablosunda bulunan verileri silme gösterilmektedir. Oluşturulan kodun veritabanı tablosundaki sütunlarla karşılık gelir.
> 
> Bu serinin parçası sunu geliştirmek için otomatik olarak oluşturulan görünümleri değiştirme odaklanır.


## <a name="add-enrolled-courses-to-student-details"></a>Kayıtlı kurslar Öğrenci Ayrıntıları Ekle

Oluşturulan kod, uygulamanız için iyi bir başlangıç noktası sunar, ancak bu mutlaka uygulamanızda gereken işlevselliğin tamamını sağlamaz. Uygulamanızı belirli gereksinimlerini karşılamak için kodu özelleştirebilirsiniz. Şu anda, uygulamanız için seçilen Öğrenci kayıtlı kurslar görüntülemez. Bu bölümde, her Öğrenci için kayıtlı kurslar ekleyecek **ayrıntıları** Öğrenci için görünümü.

Açık **Students/Details.cshtml**ve son aşağıda &lt;/dl&gt; sekmesi, ancak kapatmadan önce &lt;/div&gt; etiketi, aşağıdaki kodu ekleyin.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Bu kod kayıt tabloda seçilen Öğrenci için her kayıt için bir satır görüntüleyen bir tablo oluşturur. **Görüntü** yöntemi HTML ifadeyi temsil eden nesne için (ModelItem) oluşturur. Görüntüleme yöntemi kullanın (yalnızca özellik değeri kodda katıştırma) yerine değeri, türü ve şablon türü için doğru temelinde biçimlendirildiğini doğrulayın. Bu örnekte, her ifade döngüsünde geçerli kayıttaki tek bir özellik döndürür ve metin olarak işlenir ilkel türler değerlerdir.

Öğrenciler/dizin görünümüne yeniden göz atın ve seçim **ayrıntıları** Öğrenciler biri için. Kayıtlı kurslar görünüme dahil görürsünüz.

![kayıt ile Öğrenci](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Önceki](changing-the-database.md)
> [sonraki](enhancing-data-validation.md)
