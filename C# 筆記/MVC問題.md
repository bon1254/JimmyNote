### **1.MVC中的TempData、ViewBag、ViewData區別?**

TempData 保存在Session中，Controller每次執行請求的時候，會從Session中先獲取TempData，而後清除Session，獲取完TempData數據，雖然保存在內部字典物件中，但是其集合中的每個條目訪問一次後就從字典表中刪除，ViewData存的是Key/Value字典，使用時需要型別轉換。

ViewBag和ViewData只在當前Action中有效，等同於View，ViewBag比ViewData慢，ViewBag存dynamic類型數據，使用時不需要型別轉換

ViewData和ViewBag 中的值可以互相訪問，因為ViewBag的實作中包含了ViewData，ViewData存的是Key/Value字典，使用時需要型別轉換

### **2.闡述下MVC框架的機制，各模組的作用?**

所謂模型，就是MVC需要提供的資料來源，負責資料的存取與維護。

所謂視圖，就是用來顯示模型中資料的使用者介面。

所謂控制器，就是用來處理使用者的輸入，負責改變模型的狀態並選擇適當的視圖來顯示模型的資料。

### **[http:// 3.ASP.NET](https://link.zhihu.com/?target=http%3A//3.ASP.NET)和[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)MVC的關係?**

[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)MVC是在核心[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)基礎之上建構的，從mvc命名空間System.Web.Mvc就能看出，因為System.Web是[http:// Asp.NET](https://link.zhihu.com/?target=http%3A//Asp.NET)的核心命名空間。

如[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)MVC依賴HttpHandler，關於請求是怎麼進入控制器的，其實就是用到了HttpHandler

Session、Cookie、Cache和Application這些[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)的物件保存機制在MVC中仍是需要用到的

HttpContext、Request、Response、Server物件在MVC中仍然可以使用，在Controller中透過智慧感知的形式很容易得到這些對象

### **4. MVC對[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)好處在哪裡？**

提供非常清楚的成績管理，像是ui層，也就是view, 資料層model和管理階層controller。

單元測試比較容易。

改善了資料模型和視圖的重用性。

程式碼的結構更加優化。

### **5.什麼是razor view engine?**

這個引擎提供了資料綁定的顯示模板。

```text
@model MvcStore.Models.Customer

@{ViewBag.Title="Get Customers";}

<div class="cust"><h3><em>@Model.CustomerName</em></h3></div>
```

### **6.view bag 和view data之間的差別是什麼？**

view bag是view data的一個擴充。擴充以後可以建立動態的屬性。這樣的好處有:

不需要進行類型的轉換。

我們可以使用dynamic關鍵字。

但有一個缺點就是view bag要比view data慢一點。

### **7.解釋一下sections?**

Sections是html頁面的一部分。

```text
@rendersection("testsection")
```

在子頁面中我們定義如下的sections。

```text
@section testsection {
<h1>test content</h1>
}
```

如果這個section沒有定義的話會出錯，我們可以使用一個required屬性來防止頁面出錯。

```text
@rendersection("testsection", required: false)
```

### **8.為什麼要使用html.partial？**

這個方法用來顯示html string指定的某塊視圖。

```text
html.partial("testpartialview")
```

### **9.什麼是partial view？**

Partial view相當於傳統網頁表格中的user controls.

它的主要目的是為了重複使用這些視圖，他們一般被放在一個共用資料夾裡面。

```text
html.partial()

html.renderpartial()
```

### **10.MVC同時適用於Windows應用程式和Web應用程式嗎?**

相較於Windows應用，MVC架構更適用於Web應用。對於Windows應用，MVP(Model View Presenter )架構較好一點。如果你使用WPF和Silverlight，MVVM比較適合。

### **11.在MVC中如何保持Sessions?**

可以透過三種方式保持： tempdata, viewdata, 和viewbag。

### **12.已經有了ASPX，為什麼還要Razor?**

比起ASPX，Razor是一個乾淨的、輕量級的和語法更簡單。例如，ASPX去顯示時間：

```text
<%=DateTime.Now%>
```

在Razor中，只需要一行：

```text
@DateTime.Now
```

### **13.在MVC中如何去執行Windows認證？**

你需要修改web.config文件，並設定驗證模式為Windows。

```text
<authentication mode="Windows"/>
<authorization>
<deny users="?"/>
</authorization>
```

然後在controlle或action中，你可以使用Authorize 屬性，指定哪個使用者可以存取這個controller或action。下面的程式碼設定到只有指定的使用者可以存取它。

```text
[Authorize(Users= @"WIN-3LI600MWLQN\Administrator")]
public class StartController : Controller
{
    //
    // GET: /Start/
    [Authorize(Users = @"WIN-3LI600MWLQN\Administrator")]
    public ActionResult Index()
    {
        return View("MyView");
    }
}
```

### **14.在MVC中如何用表單認證？**

表單認證和[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)的表單驗證一樣。第一步是設定認證模式為Forms。 loginUrl是指向到controller，而不是一個頁面。

```text
<authentication mode="Forms">
<forms loginUrl="~/Home/Login"  timeout="2880"/>
</authentication>
```

我們也需要建立一個controller，去驗證使用者。如果驗證通過，需要設定cookies值。

```text
public ActionResult Login()
{
    if ((Request.Form["txtUserName"] == "Shiv") && 
          (Request.Form["txtPassword"] == "Shiv@123"))
    {
        FormsAuthentication.SetAuthCookie("Shiv",true);
        return View("About");
    }
    else
    {
        return View("Index");
    }
}
```

其它需要驗證的action都需要加一個Authorize 屬性，如果使用者沒有權限將轉向到登陸頁面。

```text
[Authorize]
PublicActionResult Default()
{
return View();
}
[Authorize]
publicActionResult About()
{
return View();
}
```

### **15.MVC有多少種不同類型的結果類型？**

注意： 很難去記住所有的12種類型。但一些重要的你可以記住，例如： ActionResult ， ViewResult ，和JsonResult 。詳情如下:

MVC中的12種結果類型，最主要的是ActionResult類，它是一個基礎類，它有11個子類型，如下：

ViewResult - 給予回應流渲染指定的視圖

PartialViewResult - 給予回應流渲染指定的局部視圖

EmptyResult - 傳回空的回應結果。

RedirectResult - 執行一個HTTP轉向到指定的URL。

RedirectToRouteResult - 執行一個HTTP轉向到一個URL，這個URL由基於路由資料的路由引擎來決定

JsonResult - 序列化一個ViewData物件到JSON格式。

JavaScriptResult - 傳回一段Javascript程式碼，它可以在客戶端執行。

ContentResult - 寫入內容到回應流，不需要視圖支援。

FileContentResult - 傳回一個檔案到客戶端。

FileStreamResult - 傳回一個檔案到客戶端，它提供的是流。

FilePathResult - 傳回一個檔案到客戶端。

### **16.什麼是WebAPI?**

HTTP是最常用的協定。過去的許多年，瀏覽器是我們使用HTTP方式公開資料的首選用戶端。但是日新月異，客戶端發展到多種形式。我們需要使用HTTP方式將資料傳遞給不同的客戶端，例如：行動手機、Javascript，Windows應用等等。

WebAPI是一種透過HTTP方式公開資料的技術，它跟隨REST規則。

### **17.什麼是MVC中的打包也壓縮?**

打包與壓縮幫助我們減少一個頁面的請求時間，從而提高頁面執行效能。

打包如何搞高性能？

我們的專案總是需要CSS和腳本檔案。打包幫助你合併多個Javascript和css文件到單一文件，從而最小化多個請求到一個請求。

例如，包含下面的web請求到一個頁。這個頁面要求兩個Javascript文件， Javascript1.js 和Javascript2.js 。

### **18.簡述Func與Action的差別？**

Func是有回傳值的委託，Action是沒有回傳值的委託。

### **19.在專案中如何解決高並發問題？**

答：盡量使用緩存，包括用戶緩存，資訊緩存等，多花點記憶體來做緩存，可以大量減少與資料庫的交互，提高效能。

最佳化資料庫查詢語句。

優化資料庫結構，多做索引，提高查詢效率。

統計的功能盡量做緩存，或按每天一統計或定時統計相關報表，避免需要時進行統計的功能。

能使用靜態頁面的地方盡量使用，減少容器的解析（盡量將動態內容產生靜態html來顯示）。

解決以上問題後，使用伺服器叢集來解決單一台的瓶頸問題。

### **19.MVC中還有哪些註解屬性用來驗證？**

如果你要去檢查字元的長度，你可以使用`StringLength`

```text
[StringLength(160)]
public string FirstName { get; set; }
```

如果你想使用註冊表達式，你可以使用RegularExpression 。

```text
[RegularExpression(@"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}")]public string Email { get; set; }
```

如果你想檢查數字是否在一個範圍內，你可以使用Range 。

```text
[Range(10,25)]public int Age { get; set; }
```

有時你想比較兩個欄位的值，我們可以使用Compare。

```text
public string Password { get; set; }
[Compare("Password")]
public string ConfirmPass { get; set; }
```

### **20.ActionResult 和ViewResult有什麼不同?**

ActionResult 是一個抽象類別，ViewResult衍生於ActionResult 類別。 ActionResult有幾個衍生類，例如： ViewResult，JsonResult，FileStreamResult， 等等。

ActionResult 可以用來發展多型和動態動象。所以如果你動態運行不同類型的視圖，ActionResult 是最好的選擇。例如下面的程式碼，你可以看見我們有一個DynamicView。基於標記（IsHtmlView），它會傳回ViewResult 或JsonResult。

### **21.MVC中如何執行打包？**

開啟App_Start資料夾中的BundleConfig.cs

在BundleConfig.cs中，加入你想打包的JS檔案路徑到打包集合。如下圖所示：

```text
bundles.Add(new ScriptBundle("~/Scripts/MyScripts").Include(
"~/Scripts/*.js")); 
```

下面是BundleConfig.cs 檔案的程式碼：

```text
  public  class BundleConfig
{
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/Scripts/MyScripts").Include(
           "~/Scripts/*.js"));
        BundleTable.EnableOptimizations = true;
    }
}
```

一旦你合併了腳本到一個文件，你可以使用下面的程式碼去呼叫它：

```text
<%= Scripts.Render("~/Scripts/MyScripts")  %>
```

### **22.MVC的路由選擇是什麼?**

路由選擇功能幫你定義一個URL規則，映射URL到控制器。

### **23.在哪裡寫路由映射表？**

在“global.asax” 文件。

### **24.在MVC中提到Area的好處?**

在MVC中Area的好處如下：

它允許我們將模型、視圖和控制器組織成應用程式的單獨功能部分，如管理、計費，客戶支援和更多。

很容易與另一個創建的其他區域整合。

也很容易進行單元測試。

### **25.可以解釋一下MVC中的RenderBody和RenderPage嗎？**

RenderBody就像web表單中的ContentPlaceHolder。這將存在於版面配置頁中，並將呈現子頁/視圖。佈局頁將只有一個RenderBody（）方法。 RenderPage也存在於版面配置頁中，多個RenderPage（）可以存在於版面頁中。

### **26.　[http:// ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)MVC的過濾器有哪些？**

[http:// APS.NET](https://link.zhihu.com/?target=http%3A//APS.NET)MVC中（以下簡稱「MVC」）的每一個請求，都會分配給對應的控制器和對應的行為方法去處理，而在這些處理的前前後後如果想再加一些額外的邏輯處理。這時候就用到了過濾器。

MVC支援的篩選器類型有四種，分別是：Authorization(授權),Action（行為）,Result（結果）和Exception（異常）。

Authorization：此類型（或過濾器）用於限制進入控制器或控制器的某個行為方法。

Exception：用來指定一個行為，這個被指定的行為處理某個行為方法或某個控制器裡面拋出的例外。

Action：用於進入行為之前或之後的處理。

Result：用於傳回結果的之前或之後的處理。