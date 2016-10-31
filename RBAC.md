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

>SELECT cp.PrivilegeMaster,
    cp.PrivilegeMasterKey,
    cp.PrivilegeAccess,
    cp.PrivilegeAccessKey,
    sm.MenuName 
 FROM CF_Privilege AS cp
  LEFT JOIN Sys_menu AS sm ON cp.PrivilegeAccessKey = sm.MenuID AND cp.PrivilegeAccess = 'Sys_Menu'
 WHERE ((cp.PrivilegeMaster = 'CF_Role'
     AND cp.PrivilegeMasterKey
     IN (SELECT RoleID FROM CF_UserRole AS cur
       LEFT JOIN CF_User AS cu ON cur.UserID = cu.UserID WHERE cu.LoginName='test1' ))
    OR
     (cp.PrivilegeMaster = 'CF_User'
     AND cp.PrivilegeMasterKey = (SELECT UserID FROM cf_user WHERE LoginName = 'test1')))
    AND
     cp.PrivilegeOperation = 'Permit' AND cp.PrivilegeAccess = 'Sys_Menu';
###查看页面截图
![结果图](https://github.com/09143516/MIS/blob/master/images/7.PNG)

##SQL2

>SELECT cp.PrivilegeMaster，
    cp.PrivilegeMasterKey ,
    cp.PrivilegeAccess ,
    cp.PrivilegeAccessKey ,
    sb.BtnName 
 FROM cf_privilege AS cp
  LEFT JOIN Sys_Button AS sb ON cp.PrivilegeAccessKey = sb.BtnID AND cp.PrivilegeAccess = 'Sys_Button'
  LEFT JOIN Sys_Menu AS sm ON sb.MenuNo = sm.MenuNo
 WHERE ((cp.PrivilegeMaster = 'CF_Role'
     AND cp.PrivilegeMasterKey
     IN (SELECT RoleID FROM CF_UserRole AS cur
       LEFT JOIN CF_User AS cu ON cur.UserID = cu.UserID WHERE cu.LoginName='test1' ))
    OR
     (cp.PrivilegeMaster = 'CF_User'
     AND cp.PrivilegeMasterKey = (SELECT UserID FROM CF_User WHERE LoginName = 'test1')))
    AND
     cp.PrivilegeOperation = 'Permit' AND cp.PrivilegeAccess = 'Sys_Button' AND sm.MenuName = '订单';
###权限截图
![结果图](https://github.com/09143516/MIS/blob/master/images/8.PNG)