---
title: ASP.NET Core yetkilendirme giriş
author: rick-anderson
description: Yetkilendirme ve ASP.NET Core uygulamaları yetkilendirme nasıl çalıştığını temel bilgileri öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483088"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>ASP.NET Core yetkilendirme giriş

<a name="security-authorization-introduction"></a>

Yetkilendirme başvuruyor ne belirleyen işlem için bir kullanıcı yapabilirsiniz. Örneğin, belge kitaplığı oluşturmak, belge ekleme, belgeleri düzenlemek ve bunları silmek için bir yönetici kullanıcı izin. Kitaplıkla çalışırken yönetici olmayan bir kullanıcı yalnızca belgeleri okuma yetkisi.

Yetkilendirme resme ve kimlik doğrulama bağımsız gerçekleşir. Ancak, yetkilendirme kimlik doğrulama mekanizması gerektirir. Kimlik doğrulaması kullanan bir kullanıcı ascertaining işlemidir. Kimlik doğrulama geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.

## <a name="authorization-types"></a>Yetkilendirme türleri

ASP.NET Core yetkilendirme sağlayan basit, bildirim temelli [rol](xref:security/authorization/roles) ve zengin bir [ilke tabanlı](xref:security/authorization/policies) modeli. Yetkilendirme gereksinimleri ifade edilir ve gereksinimleri karşı bir kullanıcının talebi işleyicileri değerlendirin. Kesinlik temelli denetimleri basit ve kullanıcı kimliğine ve kullanıcının erişmeye çalıştığı kaynak özelliklerini değerlendirmek ilkelerindeki üzerinde temel alabilir.

## <a name="namespaces"></a>Ad Alanları

Yetkilendirme bileşenleri de dahil olmak üzere, `AuthorizeAttribute` ve `AllowAnonymousAttribute` içinde bulunan öznitelikler `Microsoft.AspNetCore.Authorization` ad alanı.

Üzerinde belgelere [basit yetkilendirme](xref:security/authorization/simple).
