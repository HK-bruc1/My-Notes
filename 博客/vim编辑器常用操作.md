# vi编辑器常用操作

- 进入编辑模式：按住insert
- 同时按下 `Esc` 键和冒号键确保你处于命令模式（Command mode）。冒号 `:` 进入命令行模式，你可以在此模式下输入各种命令。
  - 输入 `:wq` 并按下回车键。`:w` 表示保存（write），`q` 表示退出（quit）。
  - 如果你只想保存但不退出，可以输入 `:w` 并按下回车键。
  - 如果你想强制保存而不退出（如果文件是只读的或者其他原因无法正常保存），可以输入 `:w!` 并按下回车键。
  - 如果你想退出但不保存，可以输入 `:q!` 并按下回车键。