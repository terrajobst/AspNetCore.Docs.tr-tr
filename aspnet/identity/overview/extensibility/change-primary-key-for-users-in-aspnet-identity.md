---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: ASP.NET ıdentity'de kullanıcılar için birincil anahtarı değiştirme | Microsoft Docs
author: tfitzmac
description: Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları için anahtar bir dize değeri kullanır. ASP.NET Identity türünü değiştirmenizi sağlar...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2ec9894df2f9a48ef482715ce71bb09fb4758127
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754125"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>ASP.NET ıdentity'de kullanıcılar için birincil anahtarı değiştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Visual Studio 2013'te varsayılan web uygulaması için kullanıcı hesapları için anahtar bir dize değeri kullanır. ASP.NET Identity veri gereksinimlerinizi karşılamak için anahtar türünü değiştirmenize olanak tanır. Örneğin, anahtar türünü dizeden tamsayıya değiştirebilirsiniz.
> 
> Bu konuda, varsayılan web uygulaması ve kullanıcı hesap anahtarı değiştirmek için bir tamsayı başlama gösterilmektedir. Aynı değişiklikleri, projenizdeki herhangi bir türde anahtar uygulamak için kullanabilirsiniz. Varsayılan web uygulamasında bu değişiklikleri yapmak nasıl gösterir, ancak benzer değişiklikler özelleştirilmiş bir uygulama için geçerli olabilir. Bu, MVC veya Web Forms ile çalışırken, gereken değişiklikleri gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Visual Studio 2013 güncelleştirme 2 (veya üzeri)
> - ASP.NET Identity 2.1 veya üzeri


Bu öğreticideki adımları gerçekleştirmek için Visual Studio 2013 güncelleştirme 2 (veya üzeri) ve ASP.NET Web uygulaması şablondan oluşturulan bir web uygulaması olmalıdır. Güncelleştirme 3'te değiştirilmiş şablonu. Bu konu, güncelleştirme 2 ve güncelleştirme 3'te şablonu değiştirmek gösterilmektedir.

Bu konu aşağıdaki bölümleri içermektedir:

- [Kimlik kullanıcı sınıfı anahtar türünü değiştirme](#userclass)
- [Anahtar türünü kullanan özelleştirilmiş kimlik sınıfları Ekle](#customclass)
- [Anahtar türü kullanmak için bağlam sınıfı ve kullanıcı Yöneticisini Değiştir](#context)
- [Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme](#startup)
- [Güncelleştirme 2 ile MVC için anahtar türü geçirilecek AccountController değiştirme](#mvcupdate2)
- [Güncelleştirme 3 ile MVC için değişiklik AccountController ve ManageController anahtar türü geçirmek için](#mvcupdate3)
- [Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate2)
- [Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate3)
- [Uygulamayı çalıştırın](#run)
- [Diğer kaynaklar](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Kimlik kullanıcı sınıfı anahtar türünü değiştirme

ASP.NET Web uygulaması şablondan oluşturulan projenizde ApplicationUser sınıfı, kullanıcı hesapları için anahtar tamsayı kullandığını belirtin. Bir tür olan IdentityUser devralacak şekilde ApplicationUser sınıfı IdentityModels.cs içinde değiştirmek **int** TKey genel parametresi için. Henüz uygulamaya olmayan üç özelleştirilmiş sınıfı adları ayrıca geçirdiğiniz.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Anahtar türünü değiştirdiniz, ancak varsayılan olarak, uygulamanın rest hala anahtar bir dize olduğunu varsayar. Ayrıca, anahtar bir dize varsayan kod türünü açıkça belirtmeniz gerekir.

İçinde **ApplicationUser** sınıfı, değişiklik **GenerateUserIdentityAsync** yöntemine aşağıdaki vurgulanmış kodu gösterildiği gibi int, dahil etmek için. Bu değişiklik, güncelleştirme 3 şablonu ile Web formları projeleri için gerekli değildir.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Anahtar türünü kullanan özelleştirilmiş kimlik sınıfları Ekle

Diğer kimlik sınıfları IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, yine de bir dize anahtarı kullanacak şekilde ayarlanır. Anahtar için bir tamsayı belirtin. Bu sınıfların yeni sürümlerini oluşturun. Bu sınıfların kadar uygulama kodunda sağlamak gerekmez, int anahtar olarak yalnızca birincil olarak ayarlama.

Aşağıdaki sınıflar IdentityModels.cs dosyanıza ekleyin.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Anahtar türü kullanmak için bağlam sınıfı ve kullanıcı Yöneticisini Değiştir

IdentityModels.cs içinde tanımını değiştirme **ApplicationDbContext** yeni kullanılacak sınıfı özelleştirilmiş sınıfları ve **int** vurgulanan kodda gösterildiği gibi anahtar.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

ThrowIfV1Schema parametresi artık oluşturucuda geçerli değil. Oluşturucu, ThrowIfV1Schema değerine geçmiyor şekilde değiştirin.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

IdentityConfig.cs açın ve değiştirmek **ApplicationUserManger** , yeni bir kullanıcı için sınıf sınıf kalıcı veri depolamak ve **int** anahtarı.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

Güncelleştirme 3'ü şablonunda ApplicationSignInManager sınıfı değiştirmeniz gerekir.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Anahtar türü kullanmak için başlangıç yapılandırmasını değiştirme

Startup.Auth.cs içinde Onvalidateıdentity kod aşağıda vurgulandığı gibi değiştirin. GetUserIdCallback tanımı ayrıştırma dize değeri bir tamsayıya dikkat edin.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Projenizin genel uygulamasını tanımıyor, **getuserıd öğesini** yöntemi, ASP.NET Identity NuGet paketini 2.1 sürümüne güncelleştirmeniz gerekebilir

ASP.NET Identity tarafından kullanılan altyapı sınıflar için çok sayıda değişiklik yapıldı. Projeyi derlemeyi denerseniz, çok sayıda hata fark edeceksiniz. Neyse ki, kalan hatalar tüm benzerdir. Identity sınıfı anahtarı için bir tamsayı bekliyor, ancak denetleyici (veya Web formu) bir dize değeri geçiyor. Her durumda, bir dizeye ve tamsayı çağırarak dönüştürmeniz gerekir **getuserıd öğesini&lt;int&gt;**. Derleme hatası listeden çalışmak veya aşağıdaki değişiklikleri izleyin.

Oluşturmakta olduğunuz ve hangi güncelleştirme Visual Studio'da yüklediğiniz projenin türünü kalan değişiklikleri bağlıdır. Aşağıdaki bağlantılar üzerinden doğrudan ilgili bölümüne Git

- [Güncelleştirme 2 ile MVC için anahtar türü geçirilecek AccountController değiştirme](#mvcupdate2)
- [Güncelleştirme 3 ile MVC için değişiklik AccountController ve ManageController anahtar türü geçirmek için](#mvcupdate3)
- [Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate2)
- [Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Güncelleştirme 2 ile MVC için anahtar türü geçirilecek AccountController değiştirme

AccountController.cs dosyasını açın. Aşağıdaki yöntemlerden değiştirmeniz gerekebilir.

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

Artık [uygulamayı çalıştırmak](#run) ve yeni bir kullanıcı kaydı.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Güncelleştirme 3 ile MVC için değişiklik AccountController ve ManageController anahtar türü geçirmek için

AccountController.cs dosyasını açın. Aşağıdaki yöntemi değiştirmeniz gerekebilir.

**ConfirmEmail** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** yöntemi

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

ManageController.cs dosyasını açın. Aşağıdaki yöntemlerden değiştirmeniz gerekebilir.

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

**ChangePassword** yöntemi

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

Artık [uygulamayı çalıştırmak](#run) ve yeni bir kullanıcı kaydı.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Güncelleştirme 2 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme

Güncelleştirme 2 ile Web formları için şu sayfalara değiştirmeniz gerekir.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Artık [uygulamayı çalıştırmak](#run) ve yeni bir kullanıcı kaydı.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Güncelleştirme 3 ile Web formları için anahtar türü geçirmek için hesap sayfaları değiştirme

Güncelleştirme 3 ile Web formları için şu sayfalara değiştirmeniz gerekir.

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
## <a name="run-application"></a>Uygulamayı çalıştırın

Varsayılan Web uygulaması şablonu için gerekli tüm değişiklikleri tamamladınız. Uygulamayı çalıştırın ve yeni bir kullanıcı kaydedin. Kullanıcı kaydedildikten sonra AspNetUsers tablo, bir tamsayı kimlik sütunu olduğunu fark edebilirsiniz.

![Yeni birincil anahtar](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Daha önce ASP.NET Identity tablolar ile farklı bir birincil anahtar oluşturduysanız, bazı ek değişiklikler yapmanız gerekir. Mümkünse, varolan bir veritabanını silmeniz yeterlidir. Web uygulamasını çalıştırın ve yeni kullanıcı ekleme veritabanı doğru tasarım ile yeniden oluşturulur. Silme yapılamıyorsa, code first geçişleri tabloları değiştirmek için çalıştırın. Ancak, yeni tamsayı birincil anahtar veritabanı SQL kimlik özelliği olarak ayarlanır değil. Kimlik sütunu bir kimlik olarak el ile ayarlamanız gerekir.

<a id="other"></a>
## <a name="other-resources"></a>Diğer kaynaklar

- [ASP.NET Identity için Özel Depolama Sağlayıcılarına Genel Bakış](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Mevcut Bir Web Sitesini SQL Üyeliğinden ASP.NET Identity’ye Geçirme](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Üyelik ve ASP.NET ıdentity'ye kullanıcı profilleri için evrensel sağlayıcı verilerini geçirme](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Örnek uygulama](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) değiştirilen birincil anahtara sahip
