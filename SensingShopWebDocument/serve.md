## index页面
> MVC开发
* AppPodCloud/Views/Shared/_Layout.cshtml  引入css
    ```html
    <link rel="stylesheet" href="~/dist/assets/skin/default_skin/css/theme.css" asp-append-version="true" />
    <link rel="stylesheet" href="~/dist/assets/skin/default_skin/css/animate.min.css" asp-append-version="true" />
    <link rel="stylesheet" href="~/dist/assets/admin-tools/admin-forms/css/admin-forms.css" asp-append-version="true" />
    ```
* AppPodCloud/Views/Home/Index.cshtml 引入js  
    开发： 
    ```html
    <app-root></app-root>
    <script type="text/javascript" src="http://localhost:4200/inline.bundle.js"></script>
    <script type="text/javascript" src="http://localhost:4200/polyfills.bundle.js"></script>
    <script type="text/javascript" src="http://localhost:4200/scripts.bundle.js"></script>
    <script type="text/javascript" src="http://localhost:4200/vendor.bundle.js"></script>
    <script type="text/javascript" src="~/js/jsoneditor.js"></script>
    <script type="text/javascript" src="http://localhost:4200/main.bundle.js"></script>
    ```
    编译：
    ```html
    <app-root></app-root>
    <script type="text/javascript" src="~/dist/inline.bundle.js"></script>
    <script type="text/javascript" src="~/dist/polyfills.bundle.js"></script>
    <script type="text/javascript" src="~/dist/scripts.bundle.js"></script>
    <script type="text/javascript" src="~/dist/vendor.bundle.js"></script>
    <script type="text/javascript" src="~/js/jsoneditor.js"></script>
    <script type="text/javascript" src="~/dist/main.bundle.js"></script>
    ``` 
## 开发命令
> ng serve

## 编译命令
> ng build --prod --aot  
> AOT静态编译，大大缩小js编译出的文件大小 

## 编译输出文件夹
> AppClients/dist
* inline.bundle.js
* polyfills.bundle.js
* scripts.bundle.js
* vendor.bundle.js
* main.bundle.js


