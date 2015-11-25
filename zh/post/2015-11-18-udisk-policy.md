# 系统无法自动挂载U盘

### 问题描述
pcDuino8 Uno上安装的Ubuntu14.04系统，由于安全策略的设置，使得系统插入U盘后，无法自动挂载，会提示

### 解决方案

修改关于udisk的相关策略，即可解决系统无法自动挂载U盘的问题。
```bash
vim /usr/share/polkit-1/actions/org.freedesktop.udisks2.policy
```
从72行开始，将如下的安全策略设置：
```xml
    72       <allow_any>auth_admin</allow_any>
    73       <allow_inactive>auth_admin</allow_inactive>
    74       <allow_active>yes</allow_active>
    75     </defaults>
    76   </action>
```
修改成：

```xml
    72       <allow_any>yes</allow_any>
    73       <allow_inactive>yes</allow_inactive>
    74       <allow_active>yes</allow_active>
    75     </defaults>
    76   </action>
```
