---
created: 2024-04-22T23:11:56 (UTC -03:00)
tags: []
source: https://www.msftnext.com/how-to-enable-ntfs-long-paths-in-windows-10/
author: MSFTNEXT project
---

# How to Enable Long Path Support in Windows 10

> ## Excerpt
> You can enable enable Long Path Support in Windows 10. Starting in version 1607 'Anniversary Update', the 260 character limitation for NTFS

---
[Skip to content](https://www.msftnext.com/how-to-enable-ntfs-long-paths-in-windows-10/#content)

You can enable enable Long Path Support in Windows 10. Starting in version 1607 ‘Anniversary Update’, the 260 character limitation for NTFS path length issue is resolved. That path length limitation has been present on Windows since very first versions.  
By default Windows 10 has a maximum file path length of 260 characters. All Windows file systems have the concept of files and folders to access stored data. Path is a string value indicating where this data is stored. However, there is a 260 character limit for paths imposed by Windows, which includes a drive letter, a colon, a separating backslash, and a null terminator. This limitation is not for the NTFS file system, but for the legacy APIs that are used to access data. There are also workarounds, such as accessing Unicode (or “wide”) versions of Windows API functions, and prefixing the path with \\\\?\\.

At the end-user level, some users may have experienced an issue in the past where File Explorer will not allow access to a file or folder if the path is longer than 260 characters. Thinking about one of Windows’ long-term concerns, Microsoft has finally added a feature that will ultimately address the long-term 260 character limitation issue.

This tutorial will show you how to enable the Long Path support for NTFS in Windows 10.

1.  Open [command prompt as Administrator](https://www.msftnext.com/command-prompt-administrator-windows-10/).
2.  Type`reg add "HKLM\SYSTEM\CurrentControlSet\Control\FileSystem" /v LongPathsEnabled /t REG_DWORD /d 1` and hit Enter.
3.  [Restart](https://www.msftnext.com/how-to-restart-and-shutdown-windows-10-different-methods/) Windows 10.
4.  You have enabled the Long Path support in File Explorer.

From now File Explorer will be able to handle paths longer than 260 characters.  Keep in mind that the change won’t affect all apps. While File Explorer comes with full support for long paths on NTFS, other apps may have issues unless their developers inlcude support for it.

If your Windows 10 include the `gpedit.msc` tool, you can use it instead. It is only available in Pro, Education and Enterprise versions of the operating system.

### **Enable Long Paths for File Explorer in Group Policy**

1.  Press Win + R keys together on your keyboard and type: `gpedit.msc`. Hit Enter.![Run Gpedit Msc](https://www.msftnext.com/wp-content/uploads/2018/01/run-gpedit-msc.png)
2.  Expand the left panel to _Local Computer Policy > Computer Configuration > Administrative Templates > System > Filesystem_.![Enable Long Paths For File Explorer In Group Policy](https://www.msftnext.com/wp-content/uploads/2016/05/Enable-Long-Paths-for-File-Explorer-in-Group-Policy.png)
3.  Double click option **Enable Win32 long paths** in the right panel.![Enable Long Path Support in Windows 10](https://www.msftnext.com/wp-content/uploads/2016/05/Enable-Win32-long-paths.png)
4.  Restart Windows 10.

![](https://www.msftnext.com/wp-content/uploads/2021/04/avatar_user_5_1619712135-42x42.png)

## The MFTNEXT Team

The MSFTNEXT project is a small team of authors who love to engage with the latest technology and gadgets. Being passionate Windows bloggers, we are happy to help others fix their system issues. [View all posts by The MFTNEXT Team](https://www.msftnext.com/author/msftnext/)

## Post navigation

We use cookies on our website to give you the most relevant experience by remembering your preferences and repeat visits. By clicking “Accept”, you consent to the use of ALL the cookies.
