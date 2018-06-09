---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: ASP.NET kimliği kullanıcılar için birincil anahtar değiştirme | Microsoft Docs
author: tfitzmac
description: Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları anahtarının bir dize değeri kullanır. ASP.NET Identity türünü değiştirmenizi sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26563775"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>ASP.NET kimliği kullanıcılar için birincil anahtarı Değiştir
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları anahtarının bir dize değeri kullanır. ASP.NET Identity veri gereksinimlerinizi karşılamak için anahtar türü değiştirmenize olanak tanır. Örneğin, bir tamsayıya bir dizeden anahtarın türünü değiştirebilirsiniz.
> 
> Bu konu, başlama varsayılan web uygulaması ve kullanıcı hesap anahtarı tamsayıya değiştirmek gösterir. Projenizde anahtar herhangi bir türde uygulamak için aynı değişikliği kullanabilirsiniz. Varsayılan web uygulamasında bu değişiklikleri yapmak nasıl gösterir, ancak özelleştirilmiş bir uygulamaya benzer değişiklikler uygulanabilir. MVC veya Web Forms ile çalışırken gerekli değişiklikleri gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Visual Studio 2013 güncelleştirme 2 (veya üstü)
> - ASP.NET Identity 2.1 veya üzeri


Bu öğreticide adımları gerçekleştirmek için Visual Studio 2013 güncelleştirme 2 (veya üstü) ve ASP.NET Web uygulaması şablondan oluşturulan bir web uygulaması olması gerekir. Güncelleştirme 3'te değiştirilen şablonu. Bu konu, güncelleştirme 2 ve güncelleştirme 3'te şablonu değiştirmek gösterilmiştir.

Bu konu aşağıdaki bölümleri içermektedir:

- [Kimlik kullanıcı sınıfı anahtar türünü değiştirme](#userclass)
- [Anahtar türü kullanmak özelleştirilmiş kimlik sınıfları ekleme](#customclass)
- [Anahtar türü kullanılacak bağlam sınıfı ve kullanıcı yöneticisini değiştirme](#context)
- [Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme](#startup)
- [Güncelleştirme 2 ile MVC için AccountController anahtar türü geçirilecek değiştirme](#mvcupdate2)
- [Güncelleştirme 3 ile MVC için AccountController ve ManageController anahtar türü geçirilecek değiştirme](#mvcupdate3)
- [Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate2)
- [Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate3)
- [Uygulamayı çalıştırma](#run)
- [Diğer kaynaklar](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Kimlik kullanıcı sınıfı anahtar türünü değiştirme

ASP.NET Web uygulaması şablondan oluşturulan projenizde ApplicationUser sınıfı, kullanıcı hesapları için anahtar tamsayı kullandığını belirtin. Bir türü IdentityUser devralacak şekilde ApplicationUser sınıfı IdentityModels.cs içinde değiştirmek **int** TKey genel parametresi için. Henüz uygulamaya olmayan üç özelleştirilmiş sınıf adlarını ayrıca geçirdiğiniz.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Anahtar türü değiştirildi, ancak varsayılan olarak, uygulamanın geri kalanına hala bir dize anahtar olduğunu varsayar. Açıkça bir dize varsayar kod anahtarında türünü belirtmeniz gerekir.

İçinde **ApplicationUser** sınıfı, değişiklik **GenerateUserIdentityAsync** yöntemi int, aşağıda vurgulanan kodda gösterildiği gibi ekleyin. Bu değişiklik güncelleştirme 3'ü şablonuyla Web Forms projeler için gerekli değildir.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Anahtar türü kullanmak özelleştirilmiş kimlik sınıfları ekleme

Diğer kimlik sınıfları, IdentityUserRole, gibi IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore RoleStore, hala bir dize anahtarı kullanacak şekilde ayarlanır. Anahtar için bir tamsayı belirtin Bu sınıfların yeni sürümlerini oluşturun. Bu sınıfların kadar uygulama kodunda sağlamak zorunda değildir, int anahtar olarak birincil olarak yalnızca ayarlama.

Aşağıdaki sınıflar IdentityModels.cs dosyanıza ekleyin.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Anahtar türü kullanılacak bağlam sınıfı ve kullanıcı yöneticisini değiştirme

IdentityModels.cs içinde tanımını değiştirin **ApplicationDbContext** yeni kullanılacak sınıfı özelleştirilmiş sınıfları ve bir **int** vurgulanan kodda gösterildiği gibi anahtar.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

ThrowIfV1Schema parametresi artık oluşturucuda geçerli değil. ThrowIfV1Schema değeri iletmez şekilde Oluşturucusu değiştirin.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

IdentityConfig.cs açın ve değiştirme **ApplicationUserManger** , yeni bir kullanıcı için sınıf sınıfı için kalıcı veri depolamak ve bir **int** anahtar.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

Güncelleştirme 3'ü şablonunda ApplicationSignInManager sınıfı değiştirmeniz gerekir.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme

Startup.Auth.cs içinde Onvalidateıdentity kod aşağıda vurgulandığı gibi değiştirin. GetUserIdCallback tanımı bir tamsayı, dize değeri ayrıştırır dikkat edin.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Projenizi genel uygulamasını tanımıyor varsa **getuserıd öğesini** yöntemi, ASP.NET Identity NuGet paketini sürüm 2.1 güncelleştirmeniz gerekebilir

ASP.NET kimliği tarafından kullanılan altyapı sınıflar için çok fazla değişiklik yaptınız. Proje derleme çalışırsanız, çok sayıda hata fark edeceksiniz. Neyse ki, kalan tüm benzer hatalardır. Identity sınıfı anahtar için bir tamsayı bekliyor, ancak bir dize değeri denetleyici (veya Web formu) geçirme. Her durumda, bir dizeye ve tamsayı çağırarak dönüştürmeniz gerekir. **getuserıd öğesini&lt;int&gt;**. Derleme hata listesinden aracılığıyla iş veya aşağıdaki değişiklikleri uygulayın.

Kalan değişikliklerini oluşturmakta olduğunuz ve Visual Studio yüklü hangi güncelleştirme proje türüne göre değişir. Aşağıdaki bağlantılar aracılığıyla doğrudan ilgili bölümüne gidin

- [Güncelleştirme 2 ile MVC için AccountController anahtar türü geçirilecek değiştirme](#mvcupdate2)
- [Güncelleştirme 3 ile MVC için AccountController ve ManageController anahtar türü geçirilecek değiştirme](#mvcupdate3)
- [Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate2)
- [Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Güncelleştirme 2 ile MVC için AccountController anahtar türü geçirilecek değiştirme

AccountController.cs dosyasını açın. Aşağıdaki yöntemlerden değiştirmeniz gerekir.

**ConfirmEmail** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**İlişkisini** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Artık [uygulamayı çalıştırın](#run) ve yeni bir kullanıcı kaydedin.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Güncelleştirme 3 ile MVC için AccountController ve ManageController anahtar türü geçirilecek değiştirme

AccountController.cs dosyasını açın. Aşağıdaki yöntem değiştirmeniz gerekir.

**ConfirmEmail** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

ManageController.cs dosyasını açın. Aşağıdaki yöntemlerden değiştirmeniz gerekir.

**Dizin** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** yöntemleri

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** yöntemleri

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**Parola değiştirme** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Artık [uygulamayı çalıştırın](#run) ve yeni bir kullanıcı kaydedin.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme

Güncelleştirme 2 ile Web formları için şu sayfaları değiştirmeniz gerekir.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Artık [uygulamayı çalıştırın](#run) ve yeni bir kullanıcı kaydedin.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme

Güncelleştirme 3 ile Web formları için şu sayfaları değiştirmeniz gerekir.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Uygulamayı çalıştırma

Varsayılan Web uygulaması şablonu için gerekli tüm değişiklikleri tamamladınız. Uygulamayı çalıştırın ve yeni bir kullanıcı kaydedin. Kullanıcı kaydolduktan sonra AspNetUsers tablo bir tamsayıdır bir kimlik sütunu olduğunu fark edeceksiniz.

![Yeni birincil anahtar](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Daha önce ASP.NET Identity tablolar ile farklı bir birincil anahtar oluşturduysanız, bazı ek değişiklikler yapmanız gerekir. Mümkünse, varolan bir veritabanını yalnızca silin. Web uygulaması çalıştırdığınızda ve yeni bir kullanıcı hesabı veritabanı doğru tasarım ile yeniden oluşturulur. Tabloları değiştirmek için code first geçişleri çalıştırmak silme mümkün değilse. Ancak, yeni tamsayı birincil anahtar veritabanı SQL kimlik özelliği olarak ayarlanmaması. Bir kimlik olarak ID sütunu el ile ayarlamanız gerekir.

<a id="other"></a>
## <a name="other-resources"></a>Diğer kaynaklar

- [ASP.NET Identity için Özel Depolama Sağlayıcılarına Genel Bakış](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Mevcut Bir Web Sitesini SQL Üyeliğinden ASP.NET Identity’ye Geçirme](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Üyelik ve ASP.NET kimliği için kullanıcı profilleri için evrensel sağlayıcısı verileri geçirme](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Örnek uygulama](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) değiştirilen birincil anahtara sahip
