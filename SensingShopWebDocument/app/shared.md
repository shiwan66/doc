## 模块用途
> 各应用模块应引用，为通用模块，无路由无服务

## 文件分布
* ./common 通用组件文件夹
* ./layout 布局组件
* *.directive.ts 指令
* *.pipe.ts 管道

## ./common
* alert 信息提示组件
* dialog-model 弹出框组件
* holder 列表为空时的默认空图组件
* local-file-upload 多文件上传组件
* navigation 左侧导航栏
* panel 各功能模块右侧导航栏
* progress 进度组件
* search 本地关键字筛选
* search 本地分类筛选
* store 各模块图块显示
* table 各模块列表显示
* tree 树显示
* upload-tab 单文件，web资源等资源上传
* view-password 密码查看

## layout
> 布局文件
```html
<div id="main">
  <hearderNav></hearderNav>
  <asideLeft></asideLeft>
  <section id="content_wrapper">
    <header id="topbar" class="" [ngStyle]="{marginTop: '60px'}" >
      <div class="topbar-left">
        ...
      </div>
    </header>
    <router-outlet></router-outlet>
    <footer id="content-footer" class="affix">
      ...
    </footer>
  </section>
</div>
```

## 指令
* check 分组名称是否重复    
* check 结束时间不早于开始时间

## 管道
* list 排序
* search 关键字搜索
* search-type 类型搜索