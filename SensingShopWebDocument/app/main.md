## main模块为根模块
* 模块类型为NgModule  
* 在Angular中Component只能注册一次，公用组件在Shared模块内注册  
* 注册服务，可以多次注入服务。在providers中

### Code
```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { MainComponent } from './main/main.component';
import { CoreModule } from './core/core.module'
import { MainRouterModule } from './main-router.module'
import { LoginModule } from './login/login.module'
import { TestModule } from './test/test.module'
import { UserModule } from './user/user.module'
import { SoftwareModule } from './software/software.module'
import { ProductModule } from './product/product.module'
import { DeviceModule } from './device/device.module'
import { AdsModule } from './ads/ads.module'
import { StoreModule } from './store/store.module'
import { DeviceInfoModule } from './device-info/device-info.module'
import { ChartModule } from './chart/chart.module'
import { RestModule } from './rest/rest.module'
import { DeviceActionModule } from './device-action/device-action.module'
import { StaffModule } from './staff/staff.module'
import { DeveloperModule } from './developer/developer.module'
import { ApplyFormModule } from './apply-form/apply-form.module'
import { ResourceModule } from './resource/resource.module'

@NgModule({
  declarations: [
    MainComponent,
  ],
  imports: [
    BrowserModule,
    CoreModule.forRoot(),
    LoginModule,
    TestModule,
    UserModule,
    SoftwareModule,
    ProductModule,
    DeviceModule, 
    AdsModule,
    StaffModule,
    StoreModule,
    DeviceInfoModule,
    ChartModule,
    RestModule,
    DeviceActionModule,
    DeveloperModule,
    ApplyFormModule,
    ResourceModule,
    MainRouterModule
  ],
  bootstrap: [MainComponent]
})
export class MainModule { }
```

### bootstrap所有app-root根入口
```js
bootstrap: [MainComponent]
```

### 引入新模块
```js
import { TestModule } from './test/test.module'
...
imports: [
    ...
    TestModule,
    ...
]
...
export class MainModule { }
```

### Route路由
```js
//mainModule
import { MainRouterModule } from './main-router.module'
//mainRouter
RouterModule.forRoot(routes) //只有mainModule的路由才能用forRoot, 其他模块的路由只能用forChild
```

### 命令
> 新建模块
```js
ng g m xxx
```

> 新建模块路由
```js
ng g m --flat xxx/xxx-router
```

> 新建组件
```js
ng g c xxx
```
