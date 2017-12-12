---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: "Veritabanına yeni bir öğe ekleyin | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a>Veritabanına yeni bir öğe ekleyin
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Bu bölümde, yeni bir rehberi oluşturmak kullanıcılara ekleyeceksiniz. App.js içinde görünüm modeli aşağıdaki kodu ekleyin:

[!code-javascript[Main](part-9/samples/sample1.js)]

Index.cshtml içinde aşağıdaki biçimlendirme değiştirin:

[!code-html[Main](part-9/samples/sample2.html)]

İle:

[!code-html[Main](part-9/samples/sample3.html)]

Bu biçimlendirme yeni Yazar göndermek için bir form oluşturur. Yazar aşağı açılan liste değerlerini verilere için bağlı `authors` observable görünüm modeli. Diğer form girişleri için değerleri veri için bağlı `newBook` görünüm modeli özelliği.

Formdaki gönderme işleyicisi bağlı `addBook` işlevi:

[!code-html[Main](part-9/samples/sample4.html)]

`addBook` İşlevi bir JSON nesnesi oluşturmak için veri bağlama form girdi geçerli değerleri okur. JSON nesnesi yazılarını sonra `/api/books`.

>[!div class="step-by-step"]
[Önceki](part-8.md)
[sonraki](part-10.md)
