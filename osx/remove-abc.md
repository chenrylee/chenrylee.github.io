打开系统偏好设置 → 键盘 → 输入法 → 添加美国🇺🇸英文，再删除 ABC 英文。
前往 ~/Library/Preferences/ 文件夹，找到 com.apple.HIToolbox.plist 文件并用 Xcode 软件打开。
删除所有包含 KeyboardLayout Name String U.S.的文件并保存。
重启电脑。
还原
打开系统偏好设置 → 键盘 → 输入法 → 添加美国🇺🇸英文或 ABC 英文即可。

Sometimes, you just want to keep one input method, but your macOS doesn't allow you to remove the builtin ABC.  
Not a big deal, here's a instruction.  
1. Go to `~/Library/Preferences/` directory.
2. Find `com.apple.HIToolbox.plist`, and then open it with **XCode**.
3. Search and delete following content:
    > **Key**: KeyboardLayout Name  
    > **Type**: String  
    > **Value**: U.S.  

4. Reboot your computer.


### Restore
Open **System Preferences**, go to **Keyboard** --> **Input Sources**, Add `English` --> `ABC` or other input sources you like.
