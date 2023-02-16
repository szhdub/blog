# odin
## mitm, run game
```shell
# ssl cert
sudo cp -r ~/odin_tw/ex/mkcert ~/.local/share/
cd ~/odin_tw/ex/mkcert && python -m http.server
```
```txt
# 手机设置

1.浏览器访问 -> 电脑ip:8000(例如: 192.168.2.6:8000) -> 点击下载安装 rootCA.pem 
2.设置 -> 安装描述文件
3.设置 -> 通用 -> 关于本机 -> 证书信任设置 -> 打开mkcert aroma@arch
4.设置 -> wifi -> 配置ip手动
如: 
ip地址: 192.168.2.207 #手机ip
子网掩码: 255.255.0.0
路由器: 192.168.2.6 #电脑ip
5.设置 -> wifi -> 配置DNS -> 手动 -> 8.8.8.8
```
```shell
cd ~/odin_tw/ex
export https_proxy="http://127.0.0.1:12333"
export http_proxy="http://127.0.0.1:12333"
mix deps.get
./run.sh 207 # 手机ip最后一位
MitmKakao.start
```

## inject acc
```elixir
Snip.inject_acc_enable(acc, "192.168.2.207") # acc, 手机ip
Snip.inject_acc_enable
Snip.inject_acc_disable
Snip.inject_acc_clear
```

## guest acc
```elixir
# create guest
uuid = HttpRoutineGuest.create(proxy)

# login lobby
acc = MnesiaKV.get(Account, uuid)
connection = Game.Connect.login_lobby(uuid, acc.lobby_token, proxy, proxy)

# server list
{connection, list} = Game.Connect.server_list(connection)
servers_open = Enum.filter(list, &(&1.isLimitUserCreate != 1)) |> Enum.map(& &1.serverID)

# player list
{connection, pl} = Game.Connect.player_list(connection)
Socket.close connection.socket

# create char
CreateChar.create(uuid, server_id = 1024, player_index = 53, proxy, proxy, proxy)
```