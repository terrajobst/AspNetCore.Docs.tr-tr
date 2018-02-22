---
title: "Yetkilendirme giriş"
author: rick-anderson
description: "Bu belge yetkilendirme temel bir açıklamasını sağlar ve ASP.NET Core yetkilendirme nasıl ilişkili olduğu açıklanmaktadır."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>Giriş

<a name="security-authorization-introduction"></a>

Yetkilendirme başvuruyor ne belirleyen işlem için bir kullanıcı yapabilirsiniz. Örneğin, belge kitaplığı oluşturmak, belge ekleme, belgeleri düzenlemek ve bunları silmek için bir yönetici kullanıcı izin. Kitaplıkla çalışırken yönetici olmayan bir kullanıcı yalnızca belgeleri okuma yetkisi.

Yetkilendirme resme ve bağımsız olan bir kullanıcı ascertaining işlemidir kimlik doğrulaması gerçekleşir. Kimlik doğrulama geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.

## <a name="authorization-types"></a>Yetkilendirme türleri

ASP.NET Core yetkilendirme sağlayan basit, bildirim temelli [rol](roles.md) ve zengin bir [ilke tabanlı](policies.md) modeli. Yetkilendirme gereksinimleri ifade edilir ve gereksinimleri karşı bir kullanıcının talebi işleyicileri değerlendirin. Kesinlik temelli denetimleri basit ve kullanıcı kimliğine ve kullanıcının erişmeye çalıştığı kaynak özelliklerini değerlendirmek ilkelerindeki üzerinde temel alabilir.

## <a name="namespaces"></a>Ad Alanları

Yetkilendirme bileşenleri de dahil olmak üzere, `AuthorizeAttribute` ve `AllowAnonymousAttribute` içinde bulunan öznitelikler `Microsoft.AspNetCore.Authorization` ad alanı.

Üzerinde belgelere [basit yetkilendirme](xref:security/authorization/simple).
