#RBAC权限管理

##逻辑过程
>Begin
>用户登录
>If 用户的userID通过中间连接表用户—角色表与之角色roleID对应，即
>UserRole.userid= User.userid
> Role.roleId=UserRole.roleId
>Then 角色表在权限表有对应权限与之对应，同时
>CF_Role和CF_User的privilege表有对应access值连接到对应Sys_Button
>And then
>用户可以获取可以操作的权限。
>End if
>End。

##SQL1

>selecct distinct m.MenuID,m.MenuNo,m.MenuOrder,m.Name,m.MenuUrl from
>Sys_Menu m,
>CF_User r
>where
>r.LoginName='test1'
###查看页面截图
![查看页面截图](https://github.com/09143516/MIS/blob/master/images/7.JPG)
![结果图](https://github.com/09143516/MIS/blob/master/images/8.JPG)

##SQL2

>selecct distinct u.loginname,r.roleid,r.roledesc,b.btnid,b.btnname,b.btnno from
>Sys_Button b,
>CF_User u,
>CF_Role r,
>CF_UserRole ur
>where
>r.LoginName='test1'
>and ur.UserID=u.UserID
>and r.RoleID=ur.RoleID
###权限截图
![权限截图](https://github.com/09143516/MIS/blob/master/images/9.JPG)
![结果图](https://github.com/09143516/MIS/blob/master/images/10.JPG)