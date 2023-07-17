# Happy Joi, form validations

Form Validation  mövzusunu React dərslərində useForm ilə daha rahat idarə edə bilirdik. Backend tərəfdə isə biz form məlumatlarını yoxlamaq üçün **Happy Joi** npm paketindən istifadə edəcik. 
Bunun üçün əvvəlki kodumuzdan davam edək. Terminalı açıb **npm i @hapi/joi** yazıb yükləyək.
```
npm i @hapi/joi
```
Yükləndiyinə package.json faylından baxa bilərsiniz.
Əvvəlki mövzuda istifadəçi əlavə etmə, düzəliş etmə və s. öyrəndik. Lakin istifadəçini əlavə edərkən bəzi şərtlər qoymaq üçün bu paketdən istifadə edəcik.
Və paketi aktivləşdirmək üçün istifadəçilərin idarə olunduğu users.js faylına daxil olaq.
```js
// users.js faylı
const  Joi  =  require('@hapi/joi')
```
Və user-ə uyğun schema( struktur ) quraq.
```js
// users.js faylı
const  Joi  =  require('@hapi/joi')
  
const  UserSchema  = Joi.object({
	username: Joi.string().min(6).max(20).required(),
	password: Joi.string().min(9).max(25).required()
});
```
Yuxarıdakı kodda 
- min(6) - minimum 6 simvol olmalı
- max(20) - maksimum 20 simvol olmalı
- required() - bu xassə mütləq doldurulmalıdır.

Qaydalara uyğun strükturu qurduqdan sonra frontend tərəfdən gələn body-ni bizim strüktur ilə yoxlayaq.
Misal. İstifadəçi əlavə etmək istəyirik. Amma əlavə etməzdən əvvəl məlumatların düzgün olub-olmadığına baxmaq istəyirik.

O zaman users POST metoduna qayıdaq . . . 
Və ordakı kodları komentə alıb öz yoxlanış kodumuzu yazaq.


```js
// users.js faylı 
router.post("/",  async  (req,  res)  =>  {
	const  {  error  }  = UserSchema.validate(req.body);
	if (error) return res.send(error.details[0].message);
    res.send("All Good")
    
	// try {
	// const newUser = await new User({
	// username: req.body.username,
	// password: req.body.password,
	// });
	  
	// const saved = await newUser.save();
	// res.status(200).json({ message: "Z-> User was added successfully" });
	// } catch (err) {
	// res.status(400).json({ message: "Z-> User was not added!!!" });
	// }
});
```

Artıq qaydalara uyğun olmayan xassə olduqda xəbərdarlıq mesajı göndərə bilirik. Və kodumuzun son halını yazaq.
```js
// users.js faylı
router.post("/",  async  (req,  res)  =>  {
	const  {  error  }  = UserSchema.validate(req.body);
	if (error) return res.send(error.details[0].message);
	
	try  {
		const  newUser  =  await  new  User({
			username: req.body.username,
			password: req.body.password
		});
		const  saved  =  await newUser.save();
		res.status(200).json({ message:  "Z-> User was added successfully"  });
	}  
	catch (err) {
		res.status(400).json({ message:  "Z-> User was not added!!!"  });
	}
});
```
Yuxarıdakl kodda məlumatlar qaydalara uyğundursa istifadəçi əlavə olunacaq. Amma uyğun deyilsə Xəbərdarlıq mesajı göndəriləcək.

Əgər istəsək yoxlanış kodumuzu başqa bir faylda saxlaya bilərik. Gəlin validation adında bir file quraq və kodları bura daşıyaq.

```js 
// validation.js faylı
const  Joi  =  require("@hapi/joi");
  
const  userValidation  =  (data)  =>  {
	const  UserSchema  = Joi.object({
		username: Joi.string().min(6).max(20).required(),
		password: Joi.string().min(9).max(25).required()
	});
	return UserSchema.validate(data);
};
  
module.exports.userValidation = userValidation;
```
Və funksiyamızı users.js faylına çağıraq.
```js 
// users.js faylı
const  {userValidation}  =  require('../validation/index')
```
```js
// users.js faylı
router.post("/",  async  (req,  res)  =>  {
	const  {  error  }  =  userValidation(req.body);
	if (error) return res.send(error.details[0].message);
	
	try  {
		const  newUser  =  await  new  User({
			username: req.body.username,
			password: req.body.password
		});
		const  saved  =  await newUser.save();
		res.status(200).json({ message:  "Z-> User was added successfully"  });
	}  
	catch (err) {
		res.status(400).json({ message:  "Z-> User was not added!!!"  });
	}
});
```













---
Hazırladı - **Zahid Vahabzadə** 
Video dərslər üçün Youtube kanalım - **[Biraz Kod Yazaq](https://www.youtube.com/channel/UCRlKqhooswsmfkxnokxcB0g)** 
Javascript-dən digər dokumentlər üçün - **[Github kanalım](https://github.com/Zahidoz/NodeJS_Tedris)**
