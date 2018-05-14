## <a name="register-the-database-context"></a>Veritabanı bağlamı kaydetme

Bu adımda, veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. Bağımlılık ekleme (dı) kapsayıcı ile kaydedilen Hizmetleri (örneğin, DB bağlamı) denetleyicileri için kullanılabilir.

DB bağlamı için yerleşik destek kullanarak hizmet kapsayıcısı kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection). Değiştir *haline* aşağıdaki kod ile dosya:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]
::: moniker-end

Önceki kod:

* Kullanılmayan kodunu kaldırır.
* Hizmet kapsayıcıya bir bellek içi veritabanına eklenen belirtir.