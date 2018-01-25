---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: "Kullanıcıların Windows kimlik doğrulaması (C#) ile kimlik doğrulaması | Microsoft Docs"
author: microsoft
description: "Bir MVC uygulaması bağlamında Windows kimlik doğrulaması kullanmayı öğrenin. Uygulamanızın web co içinde Windows kimlik doğrulamasını etkinleştirmek öğrenin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: d52597e65272fa202ef4980924f669dcc4cec593
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="authenticating-users-with-windows-authentication-c"></a>Kullanıcıların Windows kimlik doğrulaması (C#) ile kimlik doğrulaması
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bir MVC uygulaması bağlamında Windows kimlik doğrulaması kullanmayı öğrenin. Uygulamanızın web yapılandırma dosyası içinde Windows kimlik doğrulamasının nasıl etkinleştirileceği ve IIS ile kimlik doğrulaması yapılandırma konusunda bilgi edinin. Son olarak, [Authorize] özniteliği denetleyici eylemleri belirli Windows kullanıcılar veya gruplar için erişimi kısıtlamak için nasıl kullanılacağını öğrenin.


Bu öğreticinin nasıl yararlanabilirsiniz açıklamak için hedeftir parola için Internet Information Services içinde yerleşik özellikler MVC uygulamalarınızı görünümlerde güvenliğini. Yalnızca belirli Windows kullanıcıları veya belirli bir Windows gruplarının üyesi olan kullanıcılar tarafından çağrılacak denetleyici eylemleri izin öğrenin.

Windows kimlik doğrulaması kullanarak bir şirket içi Web sitesi (intranet sitesine) oluşturma ve standart Windows kullanıcı adlarını ve parolaları Web sitesi erişirken kullanabilmek için kullanıcılarınıza istediğinizde mantıklıdır. Web sitesi (bir Internet Web sitesi) karşılıklı outwards oluşturuluyorsa form kimlik doğrulaması kullanmayı düşünün.

#### <a name="enabling-windows-authentication"></a>Windows kimlik doğrulamasını etkinleştirme

Yeni bir ASP.NET MVC uygulaması oluşturduğunuzda, Windows kimlik doğrulaması varsayılan olarak etkin değildir. Form kimlik doğrulaması, MVC uygulamaları için etkin varsayılan kimlik doğrulaması türüdür. MVC uygulamanızın web (web.config) yapılandırma dosyasını değiştirerek Windows kimlik doğrulamasını etkinleştirmeniz gerekir. Bul &lt;kimlik doğrulaması&gt; bölümünde ve bu gibi form kimlik doğrulaması yerine Windows kullanacak şekilde değiştirin:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Windows kimlik doğrulamasını etkinleştirdiğinizde, web sunucunuz kullanıcıların kimliğini doğrulamak için sorumlu olur. Genellikle, iki farklı türde oluşturma ve bir ASP.NET MVC uygulaması dağıtırken kullandığınız web sunucuları vardır.

İlk olarak, bir MVC uygulaması geliştirirken, Visual Studio ile dahil ASP.NET Geliştirme Web sunucusu kullanın. ASP.NET Geliştirme Web sunucusu, varsayılan olarak, geçerli Windows hesabının (Windows'ta oturum açmak için kullandığınız her türlü hesap) bağlamında tüm sayfaları yürütür.

ASP.NET Geliştirme Web sunucusu NTLM kimlik doğrulamasını da destekler. Çözüm Gezgini penceresinde projenizin adını sağ tıklatıp Özellikler'i seçerek, NTLM kimlik doğrulamasını etkinleştirebilirsiniz. Ardından, Web sekmesini seçin ve NTLM onay (bkz: Şekil 1).

**Şekil 1 – etkinleştirme ASP.NET Geliştirme Web sunucusu için NTLM kimlik doğrulaması**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Diğer yandan, bir üretim web uygulaması için web sunucunuz olarak IIS kullanın. IIS kimlik doğrulaması dahil olmak üzere, çeşitli türlerini destekler:

- Temel kimlik doğrulaması – HTTP 1.0 protokolünün bir parçası tanımlanır. Kullanıcı adları ve parolalar düz metin (Base64 ile kodlanmış) Internet üzerinden gönderir. -Özet kimlik doğrulaması – Internet üzerinden parola kendisi yerine bir parola karmasını gönderir. -Tümleşik Windows (NTLM) kimlik doğrulaması – windows kullanarak intranet ortamlarında kullanmak için kimlik doğrulama en iyi türü. -Sertifika kimlik – bir istemci-tarafı sertifikayla etkinleştirir. Sertifika bir Windows kullanıcı hesabına eşler.

> [!NOTE] 
> 
> Bu farklı kimlik doğrulama türleri daha ayrıntılı bir genel bakış için bkz: [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Belirli bir kimlik doğrulama türünü etkinleştirmek için Internet Information Services Yöneticisi'ni kullanabilirsiniz. Tüm kimlik doğrulama türlerini her işletim sistemi söz konusu olduğunda kullanılabilir olmadığını unutmayın. Ayrıca, IIS 7.0 ile Windows Vista kullanıyorsanız, Internet Information Services Manager'da görünmeden önce Windows kimlik doğrulaması farklı türde etkinleştirmeniz gerekir. Açık **Denetim Masası, programlar, programlar ve özellikler, kapatma Windows özelliklerini aç veya Kapat**, Internet Information Services düğümünü genişletin (bkz: Şekil 2).

**Şekil 2 – etkinleştirme Windows IIS özellikleri**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Internet Information Services'ı kullanarak etkinleştirin veya farklı tür kimlik doğrulaması devre dışı bırakın. Örneğin, Şekil 3 IIS 7.0 kullanırken anonim kimlik doğrulamasını devre dışı bırakma ve etkinleştirme tümleşik Windows (NTLM) kimlik doğrulaması gösterilmektedir.

**Şekil 3 – tümleşik Windows kimlik doğrulamasını etkinleştirme**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Windows yetkilendirme kullanıcılar ve gruplar

Windows kimlik doğrulaması etkinleştirdikten sonra denetleyicileri veya denetleyici eylemleri erişimi denetlemek için [Authorize] özniteliğini kullanabilirsiniz. Bu öznitelik, tüm MVC denetleyicisi veya belirli denetleyici eylemi için uygulanabilir.

Örneğin, İNDİS(), CompanySecrets() ve StephenSecrets() adlı üç eylem listeleme 1 giriş denetleyicisi sunar. Herkes İNDİS() eylem çağırabilirsiniz. Ancak, yalnızca Windows yerel Yöneticiler grubunun üyeleri CompanySecrets() eylem çağırabilirsiniz. Son olarak, yalnızca Windows etki alanı kullanıcısı Stephen (içinde Redmond etki alanı) adlı StephenSecrets() eylem çağırabilirsiniz.

**1 – Controllers\HomeController.cs listeleme**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Windows kullanıcı hesabı denetimi (Windows Vista veya Windows Server 2008 ile çalışırken UAC nedeniyle), yerel Yöneticiler grubunun diğer grupları farklı mı davranacak. Bilgisayarınızın UAC ayarları değiştirmediğiniz sürece [Authorize] özniteliği doğru yerel Yöneticiler grubunun bir üyesi tanımaz.


Doğru izinler olmaksızın bir denetleyici eylemini çağırmayı deneyin tam olarak ne olur etkin kimlik doğrulama türüne bağlıdır. Varsayılan ASP.NET Geliştirme Sunucusu kullanırken, sadece boş bir sayfa alın. Sayfa ile sunulan bir **401 yetkilendirilmedi** HTTP yanıtı durum.

Diğer taraftan, IIS anonim kimlik doğrulamasını devre dışı ve temel kimlik doğrulaması etkin ile kullanıyorsanız, sonra korumalı sayfa isteği her zaman bir oturum açma iletişim kutusu istemi almaya devam, (bkz: Şekil 4).

**Şekil 4 – temel kimlik doğrulaması oturum açma iletişim kutusu**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Özet

Bu öğretici, Windows kimlik doğrulaması bir ASP.NET MVC uygulaması bağlamında nasıl kullanabileceğiniz açıklanmıştır. Uygulamanızın web yapılandırma dosyası içinde Windows kimlik doğrulamasını etkinleştirmek ve IIS ile kimlik doğrulamasını yapılandırmak öğrendiniz. Son olarak, [Authorize] özniteliği denetleyici eylemleri belirli Windows kullanıcılar veya gruplar için erişimi kısıtlamak için nasıl kullanılacağı hakkında bilgi edindiniz.

>[!div class="step-by-step"]
[Önceki](authenticating-users-with-forms-authentication-cs.md)
[sonraki](preventing-javascript-injection-attacks-cs.md)
