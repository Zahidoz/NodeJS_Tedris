# Router( ), use( ),  Postman
Sonuncu dəfə sizlərlə /products və /users linklərinə GET metodu yazmışdıq. Lakin bu linklərin GET metodundan əlavə olaraq POST PUT və DELETE metodları da var. O zaman gəlin həm users, həm də products üçün 4 metodu da yazaq.

```js
// index.js faylı
const  express  =  require("express");
const  app  =  express();
  
app.get("/",  (req,  res)  =>  {
	res.json({ message:  "/ GET method activated :)"  });
});
  
// users routes
app.get("/users",  (req,  res)  =>  {
	res.json({message:' users response '})
});
app.post("/users",  (req,  res)  =>  {
	res.send(" user added ");
});
app.put("/users",  (req,  res)  =>  {
	res.send(" user changed ");
});
app.delete("/users",  (req,  res)  =>  {
	res.send(" user deleted ");
});
  
// products routes
app.get("/products",  (req,  res)  =>  {
	res.send(" products response ");
});
app.post("/products",  (req,  res)  =>  {
	res.send(" products added");
});
app.put("/products",  (req,  res)  =>  {
	res.send(" products changed");
});
app.delete("/products",  (req,  res)  =>  {
	res.send(" products deleted");
});
 
  
app.listen(3000,  ()  => console.log("z_message-> server is running PORT:3000"));
```

Lakin biz http://localhost:3000/products və http://localhost:3000/users linklərinə daxil olanda  yalnız həmin linklərə GET metodu ilə çağıra bilirik. Digər POST, PUT və DELETE tipli metodları yoxlamaq üçün **POSTMAN** istifadə etməliyik.
Google-da axtarış edərək rahatlıqla rəsmi səhifəsindən yükləyə bilərsiniz. Proqramı açdıqda ekranda Metodların linklərin və digər xassələrin olduğunu görəcəksiniz. VS Code - dakı express serveri aktiv etdikdən sonra(terminalda npm start yazaraq), POSTMAN proqramında linki və metodu qeyd edərək rahaqlıqla sorğu göndərə( request ata ) bilərsiniz.


Test edikdən sonra. Kodumuza davam edək . . . 
Hələ biz 2 modelə-( user və product modeli ) aid 4 metod yazdıq. Əgər modellər çox olsa kodlar da bir o qədər də çox olacaqdı. Lakin index.js faylında bunların hamısı xoş görünmür. ona görədə routes adında folder qurub, içərisində modelləri kateqoriyalara ayıraq.

Hal-hazırda bizdə 2 model var. user və product ona görədə fayllarımızın arasında **routes folderi**  quraq. Və folder-in daxilində Modellərə uyğun olaraq users.js və products.js  faylları quraq.

users folderinin daxilində . . . 
```js
// users.js faylı
const  router  =  require('express').Router()

  
module.exports  = router
```

yazdıqdan sonra index.js faylında users-ə aid olan metodları kəsib bura router adı ilə köçürdək.
**! ! !** bu faylda app.get( ) **yox,** router.get( ) yazacıq.

```js
// users.js faylı
const  router  =  require('express').Router()

// users routes
router.get("/", (req,  res)  =>  {
	res.json({message:" users response( GET )" });
});
router.post("/", (req,  res)  =>  {
	res.json({ message: " users added( POST )" });
});
router.put("/", (req,  res)  =>  {
	res.json({ message: " users changed( PUT )" });
});
router.delete("/", (req,  res)  =>  {
	res.json({ message: " users deleted( DELETE )" });
});
  
module.exports  = router
```

Və eyni formada products.js faylında ona uyğun router-i quraq.
```js
// products.js faylı
const  products  =  require('express').Router()

// products routes
router.get("/", (req,  res)  =>  {
	res.json({message: "products response( GET )" });
});
router.post("/", (req,  res)  =>  {
	res.json({ message: "products added( POST )" });
});
router.put("/", (req,  res)  =>  {
	res.json({ message: "products changed( PUT )" });
});
router.delete("/", (req,  res)  =>  {
	res.json({ message: "products deleted( DELETE )" });
});
  
module.exports  = router
```


bu faylları index.js-ə import edib, **app.use( )** istifadə edərək birləşdirək.
**app.use( )** - bir middleware( ara vasitədir ).  Bir fayl və ya npm paketi ilə **əlaqə qurmaq** üçündür.

```js
// index.js faylı
const  express  =  require("express");
const  app  =  express();
  
// import routes
const  usersRoute  =  require("./routes/users");
const  productsRoute  =  require("./routes/products");
  
// connect routes
app.use("/users", usersRoute);
app.use("/products", productsRoute);
  
app.get("/",  (req,  res)  =>  {
	res.json({ message:  "/ GET method activated :)"  });
});
 
 
app.listen(3000,  ()  => console.log("z_message-> server is running PORT:3000"));
```

Yuxarı kodda olan 
- **app.use("/users", usersRoute);** - o deməkdir ki, linkin path-i **/users** olanda **usersRoute** işləsin.
- **app.use("/products", productsRoute);** - o deməkdir ki, linkin path-i **/products** olanda **productsRoute** işləsin.

İstəsəniz **POSTMAN** vasitəsi ilə user və product linklərinin POST GET PUT və DELETE metodlarını yenidən yoxlayın.

Bu mövzuda kodumuzu Router vasitəsi ilə kateqoriyalara ayıraraq daha aydın və səliqəli  formada hazırladıq.


Hazırladı - **Zahid Vahabzadə** Video dərslər üçün Youtube kanalım - **[Biraz Kod Yazaq](https://www.youtube.com/channel/UCRlKqhooswsmfkxnokxcB0g)**


