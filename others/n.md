### n(node管理工具)

> 从n的源码中可以看到怎么用shell写出一套命令交互ui

```shell
display_versions() {
  enter_fullscreen
  # 检测当前版本
  check_current_version
  # 情况面板
  clear
  # 显示正在使用的版本
  display_versions_with_selected $active

  trap handle_sigint INT
  trap handle_sigtstp SIGTSTP

  while true; do
    read -n 3 c
    # 读取输入的3个字符,case进行判断为「上」「下」方向
    case "$c" in
      $UP)
      # 操作前先进行面板清理
        clear
        display_versions_with_selected $(prev_version_installed)
        ;;
      $DOWN)
        clear
        display_versions_with_selected $(next_version_installed)
        ;;
      *)
        activate $selected
        leave_fullscreen
        exit
        ;;
    esac
  done
}
# 进入全屏
enter_fullscreen() {
  tput smcup
  stty -echo # 取消回显
}
#退出全屏
leave_fullscreen() {
  tput rmcup
  stty echo # 显示回显
}）
````