# NVM， NPM 與 Node

NVM, 全名爲"Node Version Manager", 是非常好用的Node版本管理器。 

這個技術出現的原因，是因爲很多時候，我們的不同的項目，node版本是不同的。 有的是 `5.0.1`, 有的是 `6.3.2`

如果node版本不對的話，運行某個應用時，很可能會遇到各種莫名其妙的問題。 

所以，我們需要在同一臺機器上，同時安裝多個版本的node .  所以NVM應用而生，很好的幫我們解決了這個問題。

Linux/Mac下的NVM官網：https://github.com/creationix/nvm 

Windows下的NVM官網： https://github.com/coreybutler/nvm-windows

NPM, 全名爲"Node Package Manager". 是隻要安裝了node, 就會捆綁安裝這個命令。它的作用跟"ruby中的 bundler", "java中的maven"相同，
都是對第三方依賴進行管理的。


## 安裝

### Windows 下的安裝： 

1.用瀏覽器打開：  https://github.com/coreybutler/nvm-windows/releases

![windows下的下載官網](./images/nvm_install_in_windows.png)

2.點擊最新的release版本下載。 例如上圖中的 "1.1.6 中的nvm-setup.zip" 文件。

3.下載後，解壓縮這個`.zip`文件，雙擊其中的`nvm-setup.exe`文件，就可以開始我們的安裝了。如下圖：

![windows下的安裝界面1](./images/nvm_install_in_windows_open_exe.png)

4.點擊下一步，選擇接受，如下圖所示：

![windows下的安裝界面2](./images/nvm_install_in_windows_accept.png)

5.繼續下一步，選擇安裝路徑。在我的機器上，我把它安裝到 `D:\nvm`. 如下圖所示：

![windows下的安裝界面3](./images/nvm_install_in_windows_location.png)

6.繼續下一步，詢問我們把nvm這個命令的快捷方式放在哪裏。（symlink的作用同快捷方式，允許我們在任意路徑下都可以調用nvm命令）

不用修改，直接下一步，如下圖所示: 

![windows下的安裝界面4](./images/nvm_install_in_windows_symlink.png)

7.然後會彈出確認安裝頁面。我們繼續點擊下一步就可以了。

8.最後，設置環境變量：

```
NVM_HOME 		D:\nvm
NVM_SYMLINK     C:\Program Files\nodejs
```

對於環境變量的修改： 從控制面板中，進入到 “所有控制面板項目” -> “高級系統配置” -> “環境變量”. 如下圖所示：

![windows下的環境變量配置](./images/nvm_install_in_windows_variable.png)

對於PATH的修改，則是在原有的值的基礎上，添加"%NVM_HOME%, %NVM_SYMLINK%", 如下圖所示： 

![windows下的path配置](./images/nvm_install_in_windows_path.png)

### Linux,Mac下的安裝：

1.下載nvm的源代碼。 運行下面命令即可。

```
$ git clone https://github.com/creationix/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
```

2.Linux, Mac的用戶：爲腳本設置啓動時加載（對於使用windows的同學，可以直接無視第二步，到官網下載exe安裝文件就可以了）.

把下面這行代碼放到： `~/.bashrc`, 或  `~/.bash_profile`, 或 `~/.zshrc` 中。

```
$ source ~/.nvm/nvm.sh
```

## 運行

注意： 不能使用 `$ which nvm` 來驗證安裝是否成功。因爲即使成功了也不會返回結果。

直接在命令行下， 輸入 

```
$ nvm 
```

就可以了。  如果安裝成功的話，會看到一些英文。如下所示：

```
Running version 1.1.6.

Usage:

  nvm arch                     : Show if node is running in 32 or 64 bit mode.
  nvm install <version> [arch] : The version can be a node.js version or "latest" for the latest stable version....
  nvm list [available]         : List the node.js installations. Type "available" at the end to see what can be ...
  nvm on                       : Enable node.js version management.
```

## 使用NVM 來安裝或者管理node版本

1.列出所有可以安裝的node版本：

windows 下的命令：   

```
$ nvm list available
```

Linux/Mac下的命令： 

```
$ nvm list-remote
```

就可以看到可以安裝的所有版本. (下面是Windows中的例子。 Linux,mac下的也非常類似）：  

```
$ nvm list available

|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    10.5.0    |    8.11.3    |   0.12.18    |   0.11.16    |
|    10.4.1    |    8.11.2    |   0.12.17    |   0.11.15    |
|    10.4.0    |    8.11.1    |   0.12.16    |   0.11.14    |
|    10.3.0    |    8.11.0    |   0.12.15    |   0.11.13    |
|    10.2.1    |    8.10.0    |   0.12.14    |   0.11.12    |
|    10.2.0    |    8.9.4     |   0.12.13    |   0.11.11    |
|    10.1.0    |    8.9.3     |   0.12.12    |   0.11.10    |
|    10.0.0    |    8.9.2     |   0.12.11    |    0.11.9    |
|    9.11.2    |    8.9.1     |   0.12.10    |    0.11.8    |

This is a partial list. For a complete list, visit https://nodejs.org/download/release
```

2.列出本地安裝好了的版本：

```
$ nvm list
```

結果形如： 

```
$ nvm list

  * 10.5.0 (Currently using 64-bit executable)
    6.9.1
```

在上面的結果中，表示當前系統安裝了兩個node版本，一個是 "6.9.1" , 一個是 "10.5.0" . 默認的node版本是 "10.5.0"

3.安裝

選擇一個版本號，就可以安裝了。  wind

```
$ nvm install 10.5.0  
```

安裝好之後，退出命令行並重新進入即可。 

4.使用

下面的命令，是爲當前文件夾指定 node 的版本。

```
$ nvm use 10.5.0 
```

對於Linux, Mac, 如果希望爲系統全局都使用某個版本，就可以運行下面的命令：

```
$ nvm alias default 10.5.0 
```

在Linux, Mac下，還可以把它放到 `~/.bashrc` 或者 `~/.bash_profile`中。 這樣系統每次啓動，都會自動指定node作爲全局的版本。

## 刪除NVM

對於Linux, Mac, 直接手動刪掉對應的配置文件（如果有的話）即可。

- `~/.nvm`
- `~/.npm` 
- `~/.bower` 

對於Windows, 則直接在控制面板中卸載軟件就可以。 

## 對於國內用戶，加快 NVM 和 NPM 下載速度的辦法

由於某些原因，在國內連接國外的服務器會比較慢，所以我們使用下面的命令，就可以在國內的鏡像服務器下載node了。 感謝淘寶提供~ 

對於NVM (安裝不同的node時使用）, 使用 `NVM_NODEJS_ORG_MIRROR` 這個變量作爲前綴。

```
$ NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/dist nvm install
```

對於NPM (安裝某些npm第三方包時使用), 則用cnpm代替 npm 命令。 

```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

對於Linux, Mac用戶，可以通過直接創建一個 "alias" 命令： 

```
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"
```

然後，就可以通過國內的淘寶服務器來安裝node的包了，例如：

```
$ cnpm install vue-cli -g
```