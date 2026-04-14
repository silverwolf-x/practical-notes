---
date: 2026-04-14
---

## finalshell激活

使用finalshell链接远程服务器，软件简单清晰占用小，没有乱七八糟的渲染

1. 下载正版
2. 用https://github.com/LuoyeAutumn/FinalShell-Active 网页激活

具体参照下面的博客

https://cuanmu.com/blog/finalshell-offline-license-guide/

> [!note]
>
> ### [如何防止被二次校验 / 恶意回连？](https://cuanmu.com/blog/finalshell-offline-license-guide/#_2-如何防止被二次校验-恶意回连)
>
> 为避免软件连接远程服务器导致再次失效或异常，建议进行以下屏蔽操作。
>
> ```powershell
> # === 配置区域 ===
> $hostsPath = "$env:SystemRoot\System32\drivers\etc\hosts"
> 
> $domains = @(
> "www.youtusoft.com",
> "youtusoft.com",
> "hostbuf.com",
> "www.hostbuf.com",
> "dkys.org",
> "tcpspeed.com",
> "www.wn1998.com",
> "wn1998.com",
> "pwlt.wn1998.com",
> "backup.www.hostbuf.com"
> )
> 
> $ips = @(
> "101.32.72.254",
> "45.56.98.223",
> "193.9.44.7",
> "103.99.178.153",
> "47.76.185.223"
> )
> 
> # === 写入 Hosts ===
> Write-Host "正在写入 Hosts..."
> foreach ($domain in $domains) {
>     $entry = "127.0.0.1 $domain"
>     if (-not (Select-String -Path $hostsPath -Pattern $domain -Quiet)) {
>         Add-Content -Path $hostsPath -Value $entry
>     }
> }
> 
> # === 刷新 DNS ===
> ipconfig /flushdns
> 
> # === 添加防火墙规则 ===
> Write-Host "正在添加防火墙规则..."
> foreach ($ip in $ips) {
>     if (-not (Get-NetFirewallRule -DisplayName "Block_$ip" -ErrorAction SilentlyContinue)) {
>         New-NetFirewallRule -DisplayName "Block_$ip" `
>             -Direction Outbound `
>             -RemoteAddress $ip `
>             -Action Block
>     }
> }
> 
> Write-Host "✅ 完成！建议重启软件"
> ```
>
> 回滚（恢复）
>
> ```powershell
> # 删除 hosts 规则
> $hostsPath = "$env:SystemRoot\System32\drivers\etc\hosts"
> (Get-Content $hostsPath) | Where-Object {
>     $_ -notmatch "youtusoft|hostbuf|dkys|tcpspeed|wn1998"
> } | Set-Content $hostsPath
> 
> # 删除防火墙规则
> Get-NetFirewallRule | Where-Object {
>     $_.DisplayName -like "Block_*"
> } | Remove-NetFirewallRule
> 
> ipconfig /flushdns
> 
> Write-Host "已恢复"
> ```
>
> 

