> 通过在GitHub添加SSH密钥实现git免密操作

- 在本地生成ssh密钥

```bash
ssh-keygen -t rsa
```

- Github添加密钥

  `Settings`->`SSH and GPG keys`->`New SSH KEY`

  ![截屏2021-09-06 上午11.27.38](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/Github KEY添加.png)

将上一步生成的`id_rsa.pub`添加进Key

- 项目SSH地址

  ![截屏2021-09-06 上午11.35.44](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/项目SSH.png)

  ```sh
  # 在本地查看`git`地址是否与项目`SSH`相同
  git remote -v
  # 如果不同，则对地址进行修改
  git remote set-url origin git@github.com:SCUTCS-WWB/ESCCAttention.git
  ```

- Push文件

```sh
git push
```

