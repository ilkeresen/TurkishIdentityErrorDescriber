# TurkishIdentityErrorDescriber
TurkishIdentityErrorDescriber

Herkese merhabalar biraz araştırma sonucu **Validation Summary** Türkçeleştirmeyi buldum belki sizinde işinize yarar diye paylaşmak istedim :) Öncelikle TurkishIdentityErrorDescriber isminde bir class oluşturdum ve miras olarak IdentityErrorDescriber belirledim kodların tamamını github da paylaştım ulaşabilirsiniz : https://github.com/ilkeresen/TurkishIdentityErrorDescriber

![TurkishIdentityErrorDescriber.cs](https://img-c.udemycdn.com/redactor/raw/q_and_a/2022-08-08_20-37-29-6243b2435a239816c2c2c0c5fa613126.png "TurkishIdentityErrorDescriber.cs")

Daha sonrasında Startup dosyamıza ConfigureServices içerisine kodlarımızı ekliyoruz.

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContext<Context>();
            services.AddIdentity<WriterUser, WriterRole>()
                .AddEntityFrameworkStores<Context>()
                .AddErrorDescriber<TurkishIdentityErrorDescriber>();
            services.AddControllersWithViews();
        }
```
![Startup.cs](https://img-c.udemycdn.com/redactor/raw/q_and_a/2022-08-08_20-37-30-bb06b28d77c5afac93a91b322b1126a3.png "Startup.cs")

Controllerda foreach ile Errorlarımızı listeliyoruz.

```csharp
[HttpPost]
        public async Task<IActionResult> Index(UserRegisterViewModel userRegisterViewModel)
        {
            if (ModelState.IsValid)
            {
                WriterUser writerUser = new WriterUser()
                {
                    Name = userRegisterViewModel.Name,
                    Surname = userRegisterViewModel.Surname,
                    Email = userRegisterViewModel.Mail,
                    UserName = userRegisterViewModel.UserName,
                    ImageUrl = userRegisterViewModel.ImageUrl
                };

                if (userRegisterViewModel.Password == userRegisterViewModel.ConfirmPassword)
                {
                    var result = await _userManager.CreateAsync(writerUser, userRegisterViewModel.Password);

                    if (result.Succeeded)
                    {
                        return RedirectToAction("Home", "Index");
                    }
                    else
                    {
                        foreach (var item in result.Errors)
                        {
                            ModelState.AddModelError(string.Empty, item.Description);
                        }
                    }
                }
            }
            return View();
        }
```

![RegisterController.cs](https://img-c.udemycdn.com/redactor/raw/q_and_a/2022-08-08_20-37-30-7dbb15c23c796afd4c2f55fea110a6ec.png "RegisterController.cs")

View dosyamıza asp-validation-summary="ModelOnly" olan bir div ekliyoruz.

```csharp
<div class="text-danger" asp-validation-summary="ModelOnly">
</div>
```

![Index.cshtml](https://img-c.udemycdn.com/redactor/raw/q_and_a/2022-08-08_20-37-30-2205c6a80689136f5dc4db2e523708aa.png "Index.cshtml")

**Aşağıda göründüğü gibi artık mesajlarımız Türkçe oldu :)**

![/Register/Index](https://img-c.udemycdn.com/redactor/raw/q_and_a/2022-08-08_20-37-30-9b7993ae5b5a2af4799e42087f4c22fc.png "/Register/Index")

Takıldığınız sormak istediğiniz bir yer olursa ulaşabilirsiniz : https://tr.linkedin.com/in/ilker-esen
