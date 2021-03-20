# Git 在Windows下的使用

在《程序員修煉之道： 從小工到專家》這本書中，提到了一個讓程序員非常尷尬的局面： 老闆要看進度，結果程序員拿不出來，只好跟老闆撒謊： 我的代碼被貓吃了。

雖然我們的代碼不會被貓吃掉，但是幾乎每個程序員都會犯的錯誤是：在下班的時候突然不小心忘記保存，或者突然斷電，結果之前敲了幾個小時的代碼就沒了。

所以，每個程序員必須要對自己的代碼做版本控制。 

在2009年之前，國內的人大部分都用 SVN. 在2010年開始，越來越多的人開始使用Git. 

本節專門爲 Windows 程序員準備。 因爲對於 Linux 和 mac用戶來說， Git都是現成的。 一行命令搞定。 所以就不贅述了。

## 爲什麼要使用 Git Bash

因爲Git Bash 不但提供了Git， 還提供了bash , 一種非常不錯的類似於 Linux 的命令行。

在Windows環境下，命令行模式跟 Linux/Mac 是相反的. 例如：  

- Linux/Mac 下: （使用左斜線作爲路徑分隔符）

```
$ cd /workspace/happy_book_vuejs
```

- Windows 下:    （使用右斜線作爲路徑分隔符，並且要分成 C,D... 盤 )

```
C:\Users\dashi>d:                      (進入到D盤)
D:\>cd workspace\happy_book_vuejs      (進入到對應目錄)
```

只要您不是做 ".NET/微信小程序/安卓" 開發，我認爲都應該轉移到 Linux 平臺上。  原因是：

你的代碼在編譯後，會運行在 Linux + Nginx 的服務器中。

最好的辦法就是，從現在開始就適應Linux的環境。 

另外，命令行在絕大多數情況下，比“圖形化”的操作界面好用。 

## 安裝 git 客戶端

我們在 windows 下，選擇 Git Bash.  

官方網站： https://gitforwindows.org/

1.打開後，就可以看到LOGO， 如下圖所示：

![git bash logo](./images/git_bash_1.png)

下載最新版本。  我下載的是 2.16.2 

2.下載後, 運行。 可以看到歡迎頁面, 如下圖所示：

![git bash install1](./images/git_bash_setup1.png)

3.選擇next, 看到選擇安裝的內容頁面，我們就用默認的就好，如下圖所示：

![git bash install1](./images/git_bash_setup2.png)

4.選擇next, 看到選擇哪個編輯器作爲git消息編輯器，

我們可以選擇下面四個編輯器： 

- nano : 最簡單的 Linux 下的編輯器，同 windows下的記事本。 學習曲線是 0 
- vim : 需要用一輩子去學習的編輯器。編輯器之神。 也是我的最愛。
- notepad++ : 加強型記事本。 也很好用。 學習曲線0
- visual studio code: 一款IDE。 

對於新手來說，建議選擇 notepad ++, 如下圖所示： 

![git bash install1](./images/git_bash_setup3.png)	

5.選擇next, 詢問我們使用什麼風格的命令行。 

這裏建議使用默認的 “Windows Command Prompt ”就好了。  如下圖所示：

![git bash install1](./images/git_bash_setup4.png)	

6.選擇next, 詢問我們使用什麼風格的SSH連接程序。   

- OpenSSH  SSH的首選  ， 這個是Git bash 自帶的。 
- Plink    第三方用戶自己安裝的SSH連接程序。

如下圖所示：

![git bash install1](./images/git_bash_setup5.png)	

7.選擇next, 詢問我們使用什麼SSH 後端。  仍然選擇默認的 OpenSSL.  如下圖所示：

![git bash install1](./images/git_bash_setup6.png)	

8.選擇next, 詢問我們使用什麼 checkout/commit 風格。 

因爲windows跟linux 對待文件的處理是不同的， 例如回車在windows下是 \r\n, 而在linux下就是 \n .  

所以，我們這裏就選擇默認的第一項就好（用windows風格checkout, 用unix 風格commit) 。 如下圖所示： 

![git bash install1](./images/git_bash_setup7.png)	

9.選擇next, 詢問我們用什麼風格的console. （命令行）

這裏一定要選擇 "MinTTY" ，也就是類似於 Linux風格的命令行。  可以說，它就是非常著名的 Cygwin. 

如下圖所示：

![git bash install1](./images/git_bash_setup8.png)	

10.選擇next, 詢問其他配置項目。 直接選擇默認就好，如下圖所示： 

![git bash install1](./images/git_bash_setup9.png)	

接下來我們就一路next  下去。就可以安裝成功了。 如下圖所示： 

![git bash install1](./images/git_bash_setup10.png)	

## 使用 Git Bash

1.打開git bash

我們打開 "Git Bash" ， 可以看到這個頁面, 如下圖所示： 

![git bash usage11](./images/git_bash_usage1.png)	

一片空蕩蕩。 估計習慣了鼠標和我的電腦的同學會非常不習慣。 不要緊。 我們慢慢來。 

2.查看當前路徑：  pwd

輸入 `$ pwd ` 就可以知道當前位置了。

```
dashi@i5-16g MINGW64 ~
$ pwd
/c/Users/dashi
```

在上面的結果中可以看到，  

- `dashi` 是我的window 系統的用戶名 . (我的外號叫大師。 )
- `i5-16g` 是我的電腦在局域網的名字
- `MINGW64` 是操作系統的名字。 可以認爲它是跟 Linux, Windows, Mac之外的第四種操作系統。
- `$` 是命令行的前綴。 後面的 `pwd` 就是我們輸入的命令。
- `/c/Users/dashi` 就是當前位置。 這個是 Linux風格。 實際上它對應的 windows的標準路徑是： `C:\Users\dashi`

git bash 每次打開的時候，都是默認的這個“當前用戶在windows”中的用戶文件夾。 

我們在一個窗口中打開 這個路徑的話，就可以看到我的用戶文件夾了。 如下圖所示： 

![git bash用法](./images/git_bash_usage2.png)

可以看到， 我輸入的路徑是：`C:\Users\dashi`, 結果在GUI中顯示的文字是： "> 此電腦 > 本地磁盤(C:) > 用戶 > dashi"

3.切換路徑:  cd

例如，我想進入到我的工作目錄(位於 D:\workspace\happy_book_vuejs) ，繼續寫我的vuejs書，就可以這樣： 

```
dashi@i5-16g MINGW64 ~
$ cd /d/workspace/happy_book_vuejs/                     

dashi@i5-16g MINGW64 /d/workspace/happy_book_vuejs (master)
$
```

所以，大家可以看到， `D:\` 在 Git bash 中對應的地址是： `/d`  

這個就是唯一需要注意的點了。 

其他的git 的基本知識( `git clone`, `git commit`, `git push`等) 就不在本書中贅述了。 對於沒有掌握的同學，一定要趕緊學習這個技術。

