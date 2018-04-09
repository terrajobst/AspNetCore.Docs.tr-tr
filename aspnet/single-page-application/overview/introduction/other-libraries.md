---
uid: single-page-application/overview/introduction/other-libraries
title: Bir kitaplık Boşaltılan dışında biliyor musunuz? | Microsoft Docs
author: madskristensen
description: Bir kitaplık Boşaltılan dışında biliyor musunuz?
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 6ac260e88fd156bad4b414e93325d5a04c490c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="know-a-library-other-than-knockout"></a>Bir kitaplık Boşaltılan dışında biliyor musunuz?
====================
by [Mads Kristensen](https://github.com/madskristensen)

[Tek sayfa uygulama (SPA) şablonu](knockoutjs-template.md) tek sayfa uygulamaları yazma başlamak için harika bir yoludur. Şablonu kullanan [Çakıştırmaları](http://knockoutjs.com/) uygulama verilerini DOM öğesine bağlanamadı.

Ancak Boşaltılan zengin istemci uygulamaları oluşturmak için yalnızca JavaScript kitaplığı değil. Diğer kitaplıkları farklı şekillerde benzer sorunları çözün. Çeşitli topluluk tarafından oluşturulan şablonlar İndirilebilecek yaptık şekilde başka bir kitaplık tercih edebilirsiniz. Bu şablonların her biri, istemci JavaScript kitaplıklarını farklı bir karışımını kullanır.

Topluluk tarafından oluşturulmuş bir şablonu yüklemek için şablon sayfaları aşağıda listelenen ve Yükle düğmesini tıklayın birini ziyaret edin. Şablonları VSIX dosyası olarak sağlanır.

## <a name="backbonejs"></a>BackboneJS

[Backbone.js SPA şablon](../templates/backbonejs-template.md). Bu şablonu geliştirmek için ilk bir çatı sağlar bir [Backbone.js](http://backbonejs.org/) ASP.NET MVC uygulaması. Kutudan çıktığında kullanıcı kaydolma, oturum açma, parola sıfırlama ve temel e-posta şablonları ile kullanıcı onayı dahil olmak üzere temel kullanıcı oturum açma işlevselliği sağlar.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) JavaScript istemci zengin verileri yönetmek için bir açık kaynak kitaplığı. Sorgulama, önbelleğe alma, değişiklik izleme, doğrulama ve daha kolay işler. İki şablonları kolay özellik:

- [Kolay/Boşaltılan](../templates/breezeknockout-template.md) şablonu bir tek sayfa uygulaması ile kolay veri yönetimi ve Çakıştırmaları için veri bağlama için kolayca nasıl oluşturabileceğinizi gösteren Boşaltılan SPA şablon genişletir.
- [Kolay/Angular](../templates/breezeangular-template.md) şablonu kolay ancak kullanarak Boşaltılan SPA şablonuyla aynı zamanda genişletir [AngularJS](http://angularjs.org) veri bağlama, bağımlılık ekleme ve ekran Yönetimi Kitaplığı.

Ayrıca, [etkin havlu SPA şablonu](../templates/hottowel-template.md) BreezeJS kullanır.

## <a name="emberjs"></a>EmberJS

[EmberJS SPA şablon](../templates/emberjs-template.md). Bu şablonu kullanan [Ember](http://emberjs.com/), çok çeşitli zengin istemci uygulamaları oluşturmak için zorluklar çözdü güçlü bir MVC JavaScript kitaplığı.

Ember SPA şablonu EmberJS ve Handlebars şablon kullanarak Boşaltılan SPA şablonunu yeniden uygulamasıdır.

## <a name="hot-towel"></a>Sık kullanılan havlu

[Sık kullanılan havlu SPA şablon](../templates/hottowel-template.md). Bu şablon kolay, çakıştırma, RequireJS ve Twitter Bootstrap dahil olmak üzere birkaç JavaScript kitaplıklarında getirir.

Burada listelenen diğer şablonları ile karşılaştırıldığında, sık kullanılan havlu teample içinden kendi oluşturabileceğiniz daha eksiksiz bir uygulama sağlar. Dikkat edilmesi gereken daha fazla kavram vardır, ancak bunları anladığınızda, bu şablonu yalnızca ne aradığınız olabilir. Bir SPA oluşturmak istediğiniz ancak başlatmak için sık kullanılan havlu kullanmak where karar veremez ve saniye cinsinden bir SPA ve tüm araçlar gerekir üzerinde oluşturmanız gerekir.

## <a name="feature-table"></a>Özellik tablosu

Her SPA şablon tarafından sağlanan özellikleri şunlardır:


|                        | ASP.NET SPA | Omurga | Kolay/Açısal | Breeze/KO |  Ember   | Sık kullanılan havlu |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Yapılacaklar örnek       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Tam şablonu      |             | &#10003; |                |           |          | &#10003;  |
| Gezinti ve geçmişi |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Kitaplıkları        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Boşaltılan        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

