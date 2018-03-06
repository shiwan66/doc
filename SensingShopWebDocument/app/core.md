## 文件分布
* auth-guard.service
* auth.service
* core.module

### auth-guard.service
> 路由守护文件，在各个router文件里使用

### auth.service
> 用户登陆，登出。及权限管理
```typescript
    // 示例
    // 用户信息
    get userInfo(): UserInfo {
        if (tokenNotExpired("token")) {
            return JSON.parse(localStorage.getItem('currentUser')) as UserInfo;
        }
        return null;
    }
    ...
    // 分组类型
    get isStore(): boolean {
        return this.userInfo.Group.GroupType == GroupType[GroupType.Store]
    }
    ...
    // 是否支持ads
    get isAdsListEnabled(): boolean {
        return this.userInfo.Group.Claims.some(item =>  item.Type == GroupClaimsType.Permissions && item.Value == GroupClaimsString.AdsListEnabled)
    }
    ...
    //组的管理权限    
    get isGroupAdd(): boolean {
        return this.userInfo.Claims.some(item =>  item.Type == UserClaimsType.Permissions && item.Value == UserClaimsString.GroupAdd)
    }    
    get isGroupDelete(): boolean {
        return this.userInfo.Claims.some(item =>  item.Type == UserClaimsType.Permissions && item.Value == UserClaimsString.GroupDelete)
    }    
    get isGroupEdit(): boolean {
        return this.userInfo.Claims.some(item =>  item.Type == UserClaimsType.Permissions && item.Value == UserClaimsString.GroupEdit)
    }    
    get isGroupView(): boolean {
        return this.userInfo.Claims.some(item =>  item.Type == UserClaimsType.Permissions && item.Value == UserClaimsString.GroupView)
    }  
    get isGroupChange(): boolean {
        return this.isGroupAdd || this.isGroupEdit
    } 
```

### core.module
> 配置authHttp，配合localStorage进行jwt验证
```typescript
export function authHttpServiceFactory(http: Http, options: RequestOptions) {
    return new AuthHttp(new AuthConfig({
        tokenName: 'token',
        tokenGetter: (() => localStorage.getItem('token')),
        globalHeaders: [{'Content-Type':'application/json'}],
        headerName: 'Authorization',
        headerPrefix: 'Bearer',
        noJwtError: true,
        noTokenScheme: true
    }), http, options);
}
```