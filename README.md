`A huge or useful hacker wordlists, depending on how you use it.`

## 安装

### 1. 克隆项目到本地

```bash
$ mkdir -p "$HOME/hacking"
$ git clone "https://github.com/MoeruCybersec/Wordlists.git" "$HOME/hacking/wordlists"
```

### 2. 添加环境变量 (可选)

将以下内容添加到你的 shell 配置文件 (例如 `.zshrc`, `.bashrc`)

```bash
export HKWL="$HOME/hacking/wordlists"
```

## 目录

* miscellaneous
  * [backup-files](./miscellaneous/backup-files.txt)
  * [backup-files-with-path](./miscellaneous/backup-files-with-path.txt)
  * [china-test-mobile-phone-number](./miscellaneous/china-test-mobile-phone-number.txt)
  * [tlds](./miscellaneous/tlds.txt)
  * [xss_events](./miscellaneous/xss_events.txt)
  * [xss_tags](./miscellaneous/xss_tags.txt)
* password
  * [darkweb2017-top10](./password/darkweb2017-top10.txt)
  * [darkweb2017-top100](./password/darkweb2017-top100.txt)
  * [darkweb2017-top1000](./password/darkweb2017-top1000.txt)
  * [darkweb2017-top10000](./password/darkweb2017-top10000.txt)
  * [fuzzdict-top1000](./password/fuzzdict-top1000.txt)
  * [fuzzdict-top19576](./password/fuzzdict-top19576.txt)
  * [fuzzdict-top3000](./password/fuzzdict-top3000.txt)
  * [fuzzdict-top500](./password/fuzzdict-top500.txt)
  * [fuzzdict-top6000](./password/fuzzdict-top6000.txt)
  * [wifi-top447](./password/wifi-top447.txt)
  * [wifi-top4800](./password/wifi-top4800.txt)
  * [wifi-top62](./password/wifi-top62.txt)
* services
  * mqtt
    * [passwords](./services/mqtt/passwords.txt)
    * [usernames](./services/mqtt/usernames.txt)
* subdomain
  * [permutations](./subdomain/permutations.txt)
  * [subdomains](./subdomain/subdomains.txt)
  * [subdomains_huge](./subdomain/subdomains_huge.txt)
* username
  * [chinese-pinyin-top500](./username/chinese-pinyin-top500.txt)
  * [chinese-top500](./username/chinese-top500.txt)
  * [top10](./username/top10.txt)

## 外部资源

该仓库中的外部词汇表包括：

- Multipurpose
  - SecLists. https://github.com/danielmiessler/SecLists.
  - Web-Fuzzing-Box. https://github.com/gh0stkey/Web-Fuzzing-Box.
- Directory / File
  - dirb. https://github.com/v0re/dirb. 词汇表参见目录 `wordlists`.
  - dirsearch. https://github.com/maurosoria/dirsearch. 词汇表参加目录 `db`.
  - dirmap. https://github.com/H4ckForJob/dirmap. 词汇表参见目录 `data`.
- Subdomain
  - resolvers. https://github.com/trickest/resolvers.
  - n0kovo_subdomains. https://github.com/n0kovo/n0kovo_subdomains.
  - altdns. https://github.com/infosec-au/altdns. 词汇表参见文件 `words.txt`

你需要使用下面的命令更新外部词汇表：

```bash
# 更新所有外部词汇表
git submodule update --init --recursive

# 更新某个外部词汇表
reponame=<仓库名称>
cd external/$reponame && git pull && cd - &>/dev/null

# 添加新的外部词汇表
url=<仓库 URL>
repname=<仓库名称>
git submodule add $url external/$reponame
```

## 资源更新建议

DNS 解析器建议经常更新，在 Unix 操作系统中，你可以安装 [iresolver](https://github.com/yuukisec/iresolver) 之后使用下面的 shell 脚本实现这一点。

```bash
update_resolvers() {
  SUB_LISTS=~/hacking/wordlists/subdomain
  RESOLVERS=$SUB_LISTS/resolvers.txt
  RESOLVERS_URL=https://raw.githubusercontent.com/trickest/resolvers/main/resolvers.txt
  RESOLVERS_TRUSTED=$SUB_LISTS/resolvers-trusted.txt
  RESOLVERS_TRUSTED_URL=https://raw.githubusercontent.com/trickest/resolvers/main/resolvers-trusted.txt

  # Update the resolver if one day has passed since the last update
  if [[ ! -f $RESOLVERS ]] || [[ $(find "$RESOLVERS" -mtime +0) ]]; then
      echo "[INF] It’s been over a day since the last update of the DNS resolver, it will be updated soon."
      [[ -f $RESOLVERS ]] && rm $RESOLVERS
      iresolver --target $RESOLVERS_URL --threads 2000 --output $RESOLVERS
  fi

  # Update the trusted resolver if seven days have passed since the last update
  if [[ ! -f $RESOLVERS_TRUSTED ]] || [[ $(find "$RESOLVERS_TRUSTED" -mtime +6) ]]; then
      echo "[INF] It’s been over seven day since the last update of the trusted DNS resolver, it will be updated soon."
      [[ -f $RESOLVERS_TRUSTED ]] && rm $RESOLVERS_TRUSTED
      iresolver --target $RESOLVERS_TRUSTED_URL --threads 2000 --output $RESOLVERS_TRUSTED
  fi
}
```

然后，将这个函数加入到你的 `.bashrc` 或 `.zshrc` 文件中，当你需要更新 DNS 解析器时，只需要运行命令：

```
update_resolvers
```