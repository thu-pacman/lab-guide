# 实验室账户

实验室账户可以用于登录实验室的服务器、访问实验室的资源等。通常分为临时账户和 LDAP 账户。

请填写 [PACMAN 新用户账号申请问卷](https://f.kdocs.cn/g/vbykcCVa?channel=wukuwd) 向管理员申请新账号。
**任何未填写问卷的请求将不会被处理。**

## 临时账户

对于临时使用服务器的校内外成员，通常在服务器上创建本地临时账户。本地临时账户不设密码，只能使用 SSH 公钥登录特定的服务器。

服务器本地临时账户的 UID 通常在 2000 到 3000 之间。在同一个集群内，可以使用脚本同步账户文件。
在不同服务器上，管理员会尽量使得用户有一致的 UID（但无法保证）。

## LDAP 身份认证

* 地址：`ldap://172.23.1.88/`
* 服务器：`fermat`

实验室的 LDAP (Lightweight Directory Access Protocol) 服务仅供正式成员使用。
它为大部分的服务器、服务提供中心化的认证与鉴权服务。通常来说，用户可以在各处无感知地使用同一套凭据，也可以在任何服务器上进行修改。
LDAP 用户的 UID 通常从 10000 开始，新建用户时必须确保没有冲突。新用户的模板为：

```ldif
dn: cn=username,ou=Groups,dc=pacman-thu,dc=org
changetype: add
objectClass: posixGroup
cn: username
gidNumber: 19999

dn: uid=username,ou=Users,dc=pacman-thu,dc=org
changetype: add
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
objectClass: sshPublicKey
uid: username
sn: Mingcheng
cn: Mingcheng Yonghu
displayName: 用户名称
uidNumber: 19999
gidNumber: 19999
homeDirectory: /home/username
loginShell: /bin/bash
shadowLastChange: 0
sshPublicKey: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD...

dn: cn=login,ou=Groups,dc=pacman-thu,dc=org
changetype: modify
add: memberUid
memberUid: username
```

其中信息应该替换为实际的用户名、UID、公钥等，`shadowLastChange` 的初始值用于强制用户修改密码。如果用户不需要登录服务器，则可删除下面加入 `login` 组的部分。

如需将服务接入 LDAP，可参考 [此文章](https://harrychen.xyz/2021/01/17/openldap-linux-auth/)，或联系管理员。

### 管理页面

为了方便使用，PACMAN 提供了 [phpLDAPadmin](https://pacman.cs.tsinghua.edu.cn/ldap/) 管理页面，用户登录后可以查看信息、添加公钥等。**注意：此页面上不能修改密码。**

此页面还可以用检查自己的账户状态：如果用户的 `loginShell` 为 `/sbin/nologin`，说明用户已经被管理员禁止登录服务器。

### 修改密码

当首次使用 LDAP 账号时，管理员会给用户分配一个初始密码。此时，用户必须先按下面的指示为自己配置公钥，然后登录任意一台服务器（如 `jumper`）来修改密码，否则无法使用 SSH 登录。修改密码时，需要先输入当前密码，然后再输入两次新密码，最后重新登录即可。

用户可以在支持 LDAP 登录的服务器上使用 `passwd` 命令修改密码（有本地用户时除外）。服务器上有 `pam_cracklib` 对新密码进行复杂性检查；LDAP 服务器也会对密码进行复杂性检查，允许的最小长度是 12 位。当任何一个检查未通过时，修改都会失败。

!!! danger "不要随意输入密码"

    除第一次登录必须修改密码外，登录集群 **不需要也不能输入密码**。
    如果遇到相应提示，请务必谨慎，必要时请联系管理员。

### SSH 公钥管理

PACMAN 所有的服务器均不允许使用密码进行 SSH 登录，只能使用公钥。为了方便用户，PACMAN 绝大部分服务器被配置为可以从 LDAP 数据库中提取公钥，用户不需要在多台服务器上放置多份公钥。

配置方式：

1. 使用 LDAP 用户名和密码登录 <https://pacman.cs.tsinghua.edu.cn/ldap/>。
2. 在左侧树中展开 `ou=Users`，找到自己的用户名（以 `uid=<user>` 开头）并点击。
3. （如果从未添加过公钥）在右侧上面的功能区点击“增加新的属性”，并选择 `sshPuclicKey`。
4. 在 `sshPublicKey` 属性对应的文本框中增加自己的公钥，格式与 `authorized_keys` 相同，每行一个。
5. 滚动到页面底部，点击 "Update Object" 按钮。
6. **（重要）** 在确认页面上再次点击 "Update Object" 按钮（注意网页可能很宽，需要滚动）。
7. 检查 `sshPublicKey` 文本框的内容是否正确。

配置完成后，用户应当可以在所有支持 LDAP 登录的服务器上使用自己的私钥登录。

!!! danger "不要在集群上放置私钥"

    如非必要，**不要在集群上生成和放置 SSH key，更不能把自己的私钥复制到集群上（换句话说，不要留下 `id_rsa`, `id_ed25519` 等文件）**。推荐使用 SSH agent 转发本地 key 用于认证，或者使用 ProxyJump (`/-J`) 等方式连接服务器。
    Agent 的转发可以跨过多层 SSH 连接，而且不会在服务器上留下私钥，安全性更高。

    关于 SSH agent 的配置可参考 [1](https://www.linode.com/docs/guides/using-ssh-agent/) [2](https://www.ssh.com/academy/ssh/agent)。

    为了增强安全性，管理员会定期检查服务器上是否有用户放置私钥，如发现可能会做出相应的处理。