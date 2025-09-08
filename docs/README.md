<h1 align="center">[ 鉴权 ] Authorization</h1>
<p align="center">
</p><br/>



>[!WARNING|style: flat|label: 简要说明 ]
>
>- 授权的本质：<span style='color:red'>[ 根据用户特性 ] </span>对某个资源或某项操作 <span style='color:Blue'>[ 授权访问  ]</span>
>
>   在`.NET Core`中，并未对授权策略做硬性规定 <span style='color:red'>[ 因此：可以完全根据用户的任意特性授权 ]</span>
>
>   (`用户特性：性别, 年龄, 地区, 政治面貌, 部门, 角色 ...`)
>
>
>
>
><br/>
>
>- <span style='color:red'>在`.NET Core`中，应用的授权由`IAuthorizationService`服务完成，它提供了两种授权方式：</span>
>
>   [`A`] 应用授权：通过手动创建权限组对资源进行授权 <span style='color:red'>[ 需频繁创建`IAuthorizationRequirement`是一项非常繁琐的操作 ]</span>
>
>   [`B`] 策略授权：<span style='color:red'>[ 推荐方式 ] 预先注册一组策略，在需要时根据注册`Key` - 提取对应的策略组`AuthorizationPolicy`]</span>
>
>   
>
>
><br/>

![image-20250720214326616](wwwroot\DocImage\v1.0.0.png ':size=800')
